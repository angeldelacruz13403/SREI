<img src="https://r2cdn.perplexity.ai/pplx-full-logo-primary-dark%402x.png" class="logo" width="120"/>

# 

---

# Creación de la VPC

Vamos a crear un VPC con las siguientes condiciones:
Se trata de montar una infraestructura para una empresa que va a tener dos subredes, una pública y otra privada.
La zona pública deberá tener salida a Internet a través de un Gateway propio creado ad-hoc para el VPC.
El rango de direcciones del VPC será el CIDR 10.0.0.0/16 y cada una de las dos subredes pública y privada tendrán los CIDR 10.0.1.0/24, 10.0.2.0/24 respectivamente.
Las subredes públicas se llamarán publica_vpc y privada_vpc

Para crear una VPC con las condiciones especificadas, siga estos pasos:

## Creación de la VPC

1. Abra la consola de Amazon VPC en https://console.aws.amazon.com/vpc/[^4].
2. En el panel de VPC, elija "Create VPC"[^4].
3. Seleccione "VPC and more" en la sección "Resources to create"[^4].
4. Configure los siguientes ajustes:
    - Name tag: Ingrese un nombre descriptivo para su VPC
    - IPv4 CIDR block: 10.0.0.0/16[^4]

## Creación de subredes

5. Cree dos subredes:
    - Subred pública:
        - Name tag: publica_vpc
        - IPv4 CIDR block: 10.0.1.0/24
    - Subred privada:
        - Name tag: privada_vpc
        - IPv4 CIDR block: 10.0.2.0/24[^1][^2]

## Configuración de la puerta de enlace de Internet

6. Cree una puerta de enlace de Internet:
    - En el panel de navegación, elija "Internet Gateways"
    - Haga clic en "Create Internet Gateway"
    - Asigne un nombre a la puerta de enlace
    - Adjunte la puerta de enlace a su VPC recién creada[^5]

## Configuración de tablas de enrutamiento

7. Cree una tabla de enrutamiento para la subred pública:
    - En el panel de navegación, elija "Route Tables"
    - Haga clic en "Create Route Table"
    - Asigne un nombre a la tabla de enrutamiento
    - Seleccione su VPC
    - Agregue una ruta a la puerta de enlace de Internet (0.0.0.0/0 apuntando al ID de la puerta de enlace de Internet)[^5]
8. Asocie la tabla de enrutamiento pública con la subred pública (publica_vpc)[^8].
9. Asegúrese de que la subred privada (privada_vpc) esté asociada con la tabla de enrutamiento principal de la VPC, que solo tiene la ruta local por defecto[^8].

Con esta configuración, ha creado una VPC con una subred pública que tiene acceso a Internet a través de una puerta de enlace de Internet, y una subred privada sin acceso directo a Internet[^1][^2][^5].

<div style="text-align: center">⁂</div>

[^1]: https://www.youtube.com/watch?v=1Vm9CiyvzbE

[^2]: https://www.linkedin.com/pulse/vpc-con-subredes-públicas-y-privadas-josé-juver-portillo-cruz

[^3]: https://cloud.google.com/run/docs/configuring/vpc-connectors?hl=es-419

[^4]: https://docs.aws.amazon.com/es_es/AmazonRDS/latest/UserGuide/CHAP_Tutorials.WebServerDB.CreateVPC.html

[^5]: https://docs.aws.amazon.com/es_es/vpc/latest/userguide/vpc-example-private-subnets-nat.html

[^6]: https://vergaracarmona.es/wp-content/uploads/2022/09/Apuntes_AWS.pdf

[^7]: https://docs.aws.amazon.com/es_es/vpc/latest/userguide/create-vpc.html

[^8]: https://dev.to/madriz03/vpc-y-subredes-en-aws-parte-2-configuracion-de-conectividad-segura-entre-recursos-y-hacia-internet-3jm8

[^9]: https://dev.to/gabeincloud/guia-paso-a-paso-para-crear-una-vpc-en-aws-28k2

[^10]: https://aprendiendoavirtualizar.com/crear-una-subred-privada-en-aws/

