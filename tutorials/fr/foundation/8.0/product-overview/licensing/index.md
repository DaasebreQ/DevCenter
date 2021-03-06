---
layout: tutorial
title: Octroi de licence dans MobileFirst Server
weight: 4
---
<!-- NLS_CHARSET=UTF-8 -->
## Présentation
{: #overview }
IBM {{ site.data.keys.mf_server }} prend en charge deux méthodes d'octroi de licence selon le type de licences que vous avez achetées.

Si vous avez acheté des Licences perpétuelles, vous pouvez utiliser ce que vous avez acheté et vérifier votre utilisation et votre conformité sur la **page Suivi des licences** dans {{ site.data.keys.mf_console }} et dans le [rapport Suivi des licences](../../administering-apps/license-tracking/#license-tracking-report). Si vous avez acheté des Licences de jeton, configurez {{ site.data.keys.mf_server }} pour communiquer avec un serveur de licences de jeton distant.

### Licences d'application ou d'appareil adressable
{: #application-or-addressable-device-licenses }
Si vous avez acheté des licences d'application ou d'appareil adressable, vous pouvez utiliser ce que vous avez acheté et vérifier votre utilisation et votre conformité sur la page Suivi des licences dans {{ site.data.keys.mf_console }} et dans le rapport Suivi des licences.

### Licence VPC (coeurs de processeur virtuel)
{: #vpc-licensing}

Mobile Foundation est également disponible avec une licence basée sur la capacité, appelée VPC (Virtual Processor Cores - coeurs de processeur virtuel). VPC est une unité de mesure utilisée pour déterminer le coût de la licence Mobile Foundation, basée sur le nombre de coeurs disponibles. Cette mesure n'est actuellement disponible que pour Cloud Pak for Applications.

Les fonctionnalités de cet indicateur sont les suivantes :

* Les clients peuvent utiliser le nombre d'applications et d'appareils de leur choix. Ce format de licence est donc avantageux par rapport à la licence par application lorsque les déploiements des clients contiennent de nombreuses applications.

* Aligné sur d'autres produits du portefeuille et source de flexibilité pour les clients dans les déploiements cloud hybride.


### Octroi de licence de jeton
{: #token-licensing }
Dans un environnement de jeton, chaque produit consomme une valeur de jeton prédéfinie par licence, par rapport à un environnement flottant traditionnel où une quantité prédéfinie par licence est consommée. La clé de licence possède un pool de jetons à partir duquel le serveur de licences calcule les jetons qui sont réservés et restitués. Des jetons sont utilisés ou libérés lorsqu'un produit réserve ou restitue des licences à partir du serveur de licences.

Votre contrat d'octroi de licence détermine si vous pouvez utiliser l'octroi de licence de jeton, le nombre de jetons disponibles, et les caractéristiques qui sont validées par des jetons. Voir Validation de licence de jeton.

Si vous avez acquis des licences de jeton, installez une version de {{ site.data.keys.mf_server }} qui prend en charge les licences de jeton et configurez votre serveur d'applications de sorte qu'il puisse communiquer avec le serveur de jetons distant. Voir Installation et configuration pour l'octroi de licence de jeton.

Dans le cadre de l'octroi de licence de jeton, vous pouvez spécifier le type d'application de licence dans le descripteur d'application de chacune de vos applications avant de les déployer. Le type d'application de licence peut être APPLICATION ou ADDITIONAL_BRAND_DEPLOYMENT. Pour les tests, vous pouvez affecter la valeur NON_PRODUCTION au type d'application de licence. Pour plus d'informations, voir Définition des informations de licence d'application.

L'outil Rational License Key Server Administration and Reporting qui est livré avec Rational License Key Server 8.1.4.9 peut administrer et générer des rapports pour la licence utilisée par {{ site.data.keys.product }}. Vous pouvez identifier les parties pertinentes du rapport en suivant les noms d'affichage ci-dessous : **MobileFirst Platform Foundation Application** ou**MobileFirst Platform Additional Brand Deployment**. Ces noms font référence au type d'application de licence pour lequel les jetons sont consommés. Pour plus d'informations, voir [Rational License Key Server Administration](https://www.ibm.com/support/knowledgecenter/SSSTWP_8.1.4/com.ibm.rational.license.doc/topics/c_rlks_admin_tool_overview.html) et [Reporting Tool overview and Rational License Key Server Fix Pack 9 (8.1.4.9)](http://www.ibm.com/support/docview.wss?uid=swg24040300).

Pour plus d'informations sur la planification de l'utilisation de l'octroi de licence de jeton avec {{ site.data.keys.mf_server }}, voir Planification de l'utilisation de l'octroi de licence de jeton.

Pour obtenir les clés de licence pour {{ site.data.keys.product }}, vous devez accéder à IBM Rational License Key Center. Pour plus d'informations sur la génération et la gestion de vos clés de licence, voir [IBM Support - Licensing](http://www.ibm.com/software/rational/support/licensing/).
