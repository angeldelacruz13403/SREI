
# Creación de EFS, RDS e instalación de wordpress

Sigue las instrucciones de las actividades correspondientes y documentales en Github. La instancia ECS debe crearse en una subred pública y conectarse correctamente con los servicios EFS y RDS para completar la instalación de WordPress.

Para crear EFS, RDS e instalar WordPress en una instancia ECS, siga estos pasos:

## Creación de Amazon EFS

1. Abra la consola de Amazon EFS en https://console.aws.amazon.com/efs/[^1].
2. Seleccione "Crear sistema de archivos"[^1].
3. Ingrese un nombre para el sistema de archivos EFS[^1].
4. Seleccione la VPC donde se creará el sistema de archivos[^1].
5. Deje las configuraciones recomendadas por defecto y haga clic en "Crear"[^1].

## Creación de Amazon RDS

1. Abra la consola de Amazon RDS en https://console.aws.amazon.com/rds/[^3].
2. Seleccione "Crear base de datos"[^3].
3. Elija MySQL como motor de base de datos[^3].
4. Seleccione la edición y versión de MySQL que desee utilizar[^3].
5. En "Plantillas", elija "Producción" para obtener una configuración de alta disponibilidad[^3].
6. Configure el identificador de la instancia, nombre de usuario y contraseña[^3].
7. Seleccione la VPC y las subredes donde se creará la instancia RDS[^3].
8. Configure las opciones de almacenamiento y conectividad según sus necesidades[^3].
9. Haga clic en "Crear base de datos"[^3].

## Creación de la instancia ECS y configuración de WordPress

1. Cree un clúster de ECS en la subred pública de su VPC.
2. Defina una tarea de ECS para ejecutar el contenedor de WordPress.
3. Configure la tarea para usar el sistema de archivos EFS creado anteriormente como volumen persistente[^2].
4. En la definición de la tarea, especifique las variables de entorno necesarias para conectar WordPress con la base de datos RDS, incluyendo el nombre de host, nombre de usuario y contraseña[^4].
5. Lance un servicio ECS utilizando la tarea definida, asegurándose de que se ejecute en la subred pública.
6. Configure un Network Load Balancer para dirigir el tráfico a la instancia ECS[^4].
7. Cree un punto de conexión de VPC para AWS PrivateLink y asócielo con el Network Load Balancer para permitir el acceso privado a la aplicación WordPress[^4].

Una vez completados estos pasos, WordPress debería estar instalado y funcionando en su instancia ECS, conectado correctamente a EFS para el almacenamiento persistente y a RDS para la base de datos.

<div style="text-align: center">⁂</div>

[^1]: https://docs.aws.amazon.com/es_es/efs/latest/ug/creating-using-create-fs.html

[^2]: https://docs.aws.amazon.com/es_es/prescriptive-guidance/latest/patterns/synchronize-data-between-amazon-efs-file-systems-in-different-aws-regions-by-using-aws-datasync.html

[^3]: https://docs.aws.amazon.com/es_es/AmazonRDS/latest/UserGuide/USER_CreateDBInstance.html

[^4]: https://docs.aws.amazon.com/es_es/prescriptive-guidance/latest/patterns/access-container-applications-privately-on-amazon-ecs-by-using-aws-privatelink-and-a-network-load-balancer.html

[^5]: https://docs.aws.amazon.com/es_es/AmazonRDS/latest/UserGuide/oracle-efs-integration.html

[^6]: https://repost.aws/es/knowledge-center/ecs-fargate-task-database-connection

[^7]: https://www.youtube.com/watch?v=ZtU4duvRAdk

[^8]: https://docs.aws.amazon.com/es_es/AmazonECS/latest/developerguide/tutorial-efs-volumes.html

