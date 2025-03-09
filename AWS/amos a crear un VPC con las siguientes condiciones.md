

# Vamos a crear un VPC con las siguientes condiciones:

Se trata de montar una infraestructura para una empresa que va a tener dos subredes, una pública y otra privada.
La zona pública deberá tener salida a Internet a través de un Gateway propio creado ad-hoc para el VPC.
El rango de direcciones del VPC será el CIDR 10.0.0.0/16 y cada una de las dos subredes pública y privada tendrán los CIDR 10.0.1.0/24, 10.0.2.0/24 respectivamente.
Las subredes públicas se llamarán publica_vpc y privada_vpc.

Creación de EFS, RDS e instalación de wordpress
Sigue las instrucciones de las actividades correspondientes y documentales en Github. La instancia ECS debe crearse en una subred pública y conectarse correctamente con los servicios EFS y RDS para completar la instalación de WordPress.

Instrucciones de entrega práctica AWS
Añadir cómo colaborador del repositorio github en el caso de utilizar un repositorio privado a

Implementaré una solución AWS que cumpla con los requisitos descritos, utilizando las mejores prácticas de arquitectura en la nube. Aquí está el plan detallado:

## 1. Creación de VPC y Subredes (Usando Terraform)

```hcl
# main.tf
provider "aws" {
  region = "us-east-1"
}

resource "aws_vpc" "main" {
  cidr_block           = "10.0.0.0/16"
  enable_dns_hostnames = true
  tags = {
    Name = "Empresa-VPC"
  }
}

resource "aws_internet_gateway" "gw" {
  vpc_id = aws_vpc.main.id
}

# Subred Pública
resource "aws_subnet" "publica" {
  vpc_id            = aws_vpc.main.id
  cidr_block        = "10.0.1.0/24"
  availability_zone = "us-east-1a"
  tags = {
    Name = "publica_vpc"
  }
}

# Subred Privada
resource "aws_subnet" "privada" {
  vpc_id            = aws_vpc.main.id
  cidr_block        = "10.0.2.0/24"
  availability_zone = "us-east-1a"
  tags = {
    Name = "privada_vpc"
  }
}

# Tabla de Rutas Pública
resource "aws_route_table" "public" {
  vpc_id = aws_vpc.main.id

  route {
    cidr_block = "0.0.0.0/0"
    gateway_id = aws_internet_gateway.gw.id
  }
}

resource "aws_route_table_association" "public" {
  subnet_id      = aws_subnet.publica.id
  route_table_id = aws_route_table.public.id
}
```


## 2. Configuración de EFS y RDS

**EFS (Sistema de Archivos Elástico):**

```hcl
resource "aws_efs_file_system" "wordpress" {
  creation_token = "wordpress-efs"
  tags = {
    Name = "WordPress-EFS"
  }
}

resource "aws_efs_mount_target" "private" {
  file_system_id  = aws_efs_file_system.wordpress.id
  subnet_id       = aws_subnet.privada.id
  security_groups = [aws_security_group.efs.id]
}
```

**RDS (Base de Datos):**

```hcl
resource "aws_db_subnet_group" "private" {
  name       = "rds-private-subnet"
  subnet_ids = [aws_subnet.privada.id]
}

resource "aws_db_instance" "wordpress" {
  allocated_storage      = 20
  engine                 = "mysql"
  instance_class         = "db.t3.micro"
  db_name                = "wordpress"
  username               = "admin"
  password               = "SuperSecretPassword123!" # Usar Secrets Manager en producción
  db_subnet_group_name   = aws_db_subnet_group.private.name
  vpc_security_group_ids = [aws_security_group.rds.id]
  skip_final_snapshot    = true
}
```


## 3. Implementación de WordPress en ECS

**Task Definition:**

```json
{
  "family": "wordpress",
  "networkMode": "awsvpc",
  "containerDefinitions": [{
    "name": "wordpress",
    "image": "wordpress:latest",
    "portMappings": [{
      "containerPort": 80,
      "hostPort": 80
    }],
    "environment": [
      {"name": "WORDPRESS_DB_HOST", "value": "${aws_db_instance.wordpress.endpoint}"},
      {"name": "WORDPRESS_DB_USER", "value": "admin"},
      {"name": "WORDPRESS_DB_PASSWORD", "value": "SuperSecretPassword123!"}
    ],
    "mountPoints": [{
      "sourceVolume": "wordpress-data",
      "containerPath": "/var/www/html"
    }]
  }],
  "requiresCompatibilities": ["FARGATE"],
  "cpu": "256",
  "memory": "512"
}
```


## 4. Configuración de Seguridad

```hcl
# Security Group para ECS
resource "aws_security_group" "ecs" {
  vpc_id = aws_vpc.main.id

  ingress {
    from_port   = 80
    to_port     = 80
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }
}

# Security Group para RDS
resource "aws_security_group" "rds" {
  vpc_id = aws_vpc.main.id

  ingress {
    from_port       = 3306
    to_port         = 3306
    protocol        = "tcp"
    security_groups = [aws_security_group.ecs.id]
  }
}
```


## 5. Implementación en GitHub

Estructura del repositorio:

```
📁 infraestructura-aws/
├── 📁 terraform/
│   ├── main.tf
│   ├── variables.tf
│   └── outputs.tf
├── 📁 docs/
│   ├── arquitectura.png
│   └── diagrama-flujo.drawio
├── 📁 screenshots/
│   ├── vpc-creation.png
│   ├── efs-mount.png
│   └── wordpress-running.png
└── README.md
```

**Contenido del README.md:**

```markdown
# Implementación de WordPress en AWS con ECS, RDS y EFS

## Requisitos Previos
- AWS CLI configurado
- Terraform >= 1.0
- Acceso IAM con permisos adecuados

## Pasos de Implementación
1. Clonar repositorio
2. Inicializar Terraform:
```

cd terraform
terraform init
terraform apply

```
3. Verificar recursos en AWS Console
4. Acceder a WordPress via ALB DNS

## Diagrama de Arquitectura
![Arquitectura](docs/arquitectura.png)
```


## 6. Configuración de Colaboradores (Repositorio Privado)

1. Ir a Settings > Collaborators en GitHub
2. Añadir usuario `tu-colaborador` con permisos de **Write**
3. Confirmar invitación por correo electrónico

**Recomendaciones de Seguridad:**

- Usar AWS Secrets Manager para contraseñas
- Habilitar registro de CloudTrail
- Configurar rotación automática de credenciales
- Utilizar políticas IAM de mínimo privilegio

Los recursos creados cumplen con el modelo de seguridad de capas, donde:

- La subred pública solo contiene el ALB y recursos de frontera
- Los servicios críticos (RDS, EFS) están en subred privada
- Las reglas de seguridad limitan el tráfico únicamente a los puertos necesarios

Para verificar la instalación:

1. Acceder a la URL del ALB
2. Completar el asistente de instalación de WordPress
3. Subir archivos multimedia para verificar EFS
4. Realizar consultas a la base de datos para validar RDS

<div style="text-align: center">⁂</div>

[^1]: https://docs.aws.amazon.com/es_es/workspaces/latest/adminguide/create-configure-new-vpc-with-private-public-subnets-nat.html

[^2]: https://docs.aws.amazon.com/workspaces/latest/adminguide/create-configure-new-vpc-with-private-public-subnets-nat.html

[^3]: https://www.youtube.com/watch?v=ZtU4duvRAdk

[^4]: https://aprendiendoavirtualizar.com/crear-una-subred-privada-en-aws/

[^5]: https://docs.aws.amazon.com/vpc/latest/userguide/configure-subnets.html

[^6]: https://cloudiostrategy.com/wordpress-en-aws/

[^7]: https://docs.aws.amazon.com/es_es/vpc/latest/userguide/configure-subnets.html

[^8]: https://dev.to/aws-builders/secure-aws-vpc-using-public-and-private-subnets-4nih

[^9]: https://aws.amazon.com/es/blogs/aws-spanish/como-desplegar-wordpress-sobre-contenedores-en-aws-utilizando-cdk/

