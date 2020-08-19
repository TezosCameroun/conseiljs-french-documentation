# **ConseilJS**

Une bibliothèque pour construire des applications décentralisées en Typescript et Javascript, actuellement centrée sur la plateforme [Tezos](http://tezos.com/) .

ConseilJS se connecte aux nœuds Tezos pour les données et les opérations de la chaîne en direct et aux serveurs [Conseil](https://github.com/Cryptonomic/Conseil) pour des analyses haute performance sur les données de la blockchain. En interne, Cryptonomic utilise [Nautilus](https://github.com/Cryptonomic/Nautilus) pour les déploiements d'infrastructure de ces services. C'est la bibliothèque au cœur de nos produits, [Arronax](https://arronax.io/) , [Mininax](https://mininax.io/) et certainement [Galleon](https://galleon-wallet.tech/) . Il existe [également des liaisons ReasonML](https://github.com/Cryptonomic/ConseilJS-ReasonML-Bindings) .

Cryptonomic propose un service d'infrastructure - [Nautilus Cloud](https://nautilus.cloud/) qui permet un accès rapide à la plate-forme Tezos avec des produits qui facilitent sa construction.


## **[Autres ressources](https://cryptonomic.github.io/ConseilJS/#/?id=other-resources)**

**En plus de ces documents, il existe une [procédure automatisée pour les flux de travail Tezos courants](https://gist.github.com/anonymoussprocket/148d82fc9bf6c413be04155a90d57be6) . L' explorateur de blocs [Mininax](https://mininax.io/) est pratique pour visualiser les opérations soumises pendant cette exécution. Cryptonomic organise régulièrement des ateliers de développement d’application à l'échelle internationale, si vous êtes intéressé par un, contactez-le pour l'organiser. Le matériel pour ces événements est continuellement mis à jour et [est disponible](https://medium.com/the-cryptonomic-aperiodical/blockchain-development-with-tezos-698aa930e50f) sur [Medium](https://medium.com/the-cryptonomic-aperiodical) .**

## **[Utiliser avec Nodejs](https://cryptonomic.github.io/ConseilJS/#/?id=use-with-nodejs)**

**Ajoutez notre [package NPM](https://www.npmjs.com/package/conseiljs) à votre projet et une bibliothèque de signature. Depuis ConseilJS 5.0.0, la fonctionnalité de signature cryptographique est externe à la bibliothèque principale. De plus, il est nécessaire de fournir un enregistreur et de récupérer les références comme spécifié ci-dessous. Ceci est fait séparément pour permettre l'utilisation de la [récupération prenant en charge le proxy](https://www.npmjs.com/package/socks5-node-fetch) par exemple.**

    npm i conseiljs
    npm i conseiljs-softsigner

> import fetch from 'node-fetch';
> 
> import * as log from 'loglevel';
> 
> import { registerFetch, registerLogger, Signer, TezosMessageUtils }
> from 'conseiljs';
> 
> import { KeyStoreUtils, SoftSigner } from 'conseiljs-softsigner';
> 
> const logger = log.getLogger('conseiljs');
> 
> logger.setLevel('debug', false); // to see only errors, set to 'error'
> 
> registerLogger(logger);
> 
> registerFetch(fetch);
> 
> let signer: Signer;
> 
> const keyStore = await
> KeyStoreUtils.restoreIdentityFromSecretKey('edskRgu8wHxjwayvnmpLDDijzD3VZDoAH7ZLqJWuG4zg7LbxmSWZWhtkSyM5Uby41rGfsBGk4iPKWHSDniFyCRv3j7YFCknyHH');
> 
> signer = await
> SoftSigner.createSigner(TezosMessageUtils.writeKeyWithHint(keyStore.secretKey,
> 'edsk'), -1);

## **[Utiliser avec le Web](https://cryptonomic.github.io/ConseilJS/#/?id=use-with-web)**

Contrairement à l'exemple nodejs, il n'est pas possible de configurer des références de récupération ou de journalisation pour la version Web de ConseilJS.

    <html>
    <head>
    <script src="https://cdn.jsdelivr.net/gh/cryptonomic/conseiljs/dist-web/conseiljs.min.js"integrity="sha384-IufW5flMmPmFPiUm4GlKLocLuEWZlGoCA5ukejQgI66LcXELSsraBP7dux+BE9Ds"crossorigin="anonymous"></script><script src="https://cdn.jsdelivr.net/gh/cryptonomic/conseiljs-softsigner/dist-web/conseiljs-softsigner.min.js"crossorigin="anonymous"></script> <script>
    const keyStore = await conseiljssoftsigner.KeyStoreUtils.restoreIdentityFromSecretKey('edskRgu8wHxjwayvnmpLDDijzD3VZDoAH7ZLqJWuG4zg7LbxmSWZWhtkSyM5Uby41rGfsBGk4iPKWHSDniFyCRv3j7YFCknyHH');
    signer = await conseiljssoftsigner.SoftSigner.createSigner(TezosMessageUtils.writeKeyWithHint(keyStore.secretKey, 'edsk'), -1);
    </script>
    </head>
    <body>
    ...
    </body>
    </html>

Si vous utilisez le mécanisme de vérification d'intégrité, assurez-vous de garder le hachage à jour. La référence ci-dessus indique le [tronc ConseilJS sur GitHub](https://github.com/Cryptonomic/ConseilJS/blob/master/dist-web/conseiljs.min.js) et il est mis à jour lorsque de nouvelles versions sont effectuées, ce qui rendra les hachages précédents invalides et rendra votre page Web non fonctionnelle. Pour cette raison, les exemples de code suivants n'incluent pas l'attribut integrity.
# **[Présentation et exemples de l'API](https://cryptonomic.github.io/ConseilJS/#/?id=api-overview-and-examples)**
## **[Développement de contrat Lightning Route](https://cryptonomic.github.io/ConseilJS/#/?id=contract-development-lightning-route)**

Si vous souhaitez passer directement au travail sur les contrats intelligents Michelson, suivez simplement ces instructions dans l'ordre suivant:

1.  [Créer un projet de n](https://nodejs.org/en/docs/guides/getting-started-guide/)ode
    
2.  [Install ConseilJS](https://cryptonomic.github.io/ConseilJS/#/?id=use-with-nodejs)
    
3.  [Obtenez une clé API](https://cryptonomic.github.io/ConseilJS/#/?id=api-key)
    
4.  [Créer un compte Tezos testnet](https://cryptonomic.github.io/ConseilJS/#/?id=create-an-tezos-testnet-account)
    
5.  [Initialiser le compte](https://cryptonomic.github.io/ConseilJS/#/?id=initialize-the-account)
    
6.  [Déployer un contrat](https://cryptonomic.github.io/ConseilJS/#/?id=deploy-a-contract)
    
7.  [Invoquer un contrat](https://cryptonomic.github.io/ConseilJS/#/?id=invoke-a-contract)

## **[Blockchain Analytics Lightning Route](https://cryptonomic.github.io/ConseilJS/#/?id=blockchain-analytics-lightning-route)**

Si vous vous intéressez à la statistique et à l'intelligence d'affaires, ConseilJS dispose également d'outils pour cela. Alors que pour une extraction de données plus lourde, l'utilisation directe de [Conseil](https://cryptonomic.github.io/Conseil) peut avoir plus de sens, mais si vous créez des outils utilisateur tels que des tableaux de bord, ConseilJS facilitera considérablement le processus.

1.  [Créer un projet de nœud](https://nodejs.org/en/docs/guides/getting-started-guide/)
    
2.  [Install ConseilJS](https://cryptonomic.github.io/ConseilJS/#/?id=use-with-nodejs)
    
3.  [Obtenez une clé API](https://cryptonomic.github.io/ConseilJS/#/?id=api-key)
    
4.  [Exécuter des rapports](https://cryptonomic.github.io/ConseilJS/#/?id=reporting--analytics-functions)

### **[clé API](https://cryptonomic.github.io/ConseilJS/#/?id=api-key)**

Certaines fonctions ConseilJS nécessitent une clé API pour une instance de service Conseil. Alors que les interactions directes en chaîne se produisent via un nœud Tezos, les opérations de lecture de Conseil comme celles de l' TezosConseilClientespace de noms nécessitent une clé API.
L'obtention d'une clé de développement est facile. Cryptonomic propose un service d'infrastructure - [Nautilus Cloud](https://nautilus.cloud/) qui permet un accès rapide à la plate-forme Tezos avec des produits qui facilitent sa construction.


### **[Opérations de la chaîne Tezos](https://cryptonomic.github.io/ConseilJS/#/?id=tezos-chain-operations)**


Pour exécuter des opérations sur la chaîne Tezos, un lien vers un nœud Tezos est nécessaire. Un lien peut être trouvé après la connexion à [Nautilus Cloud](https://nautilus.cloud/) pour le réseau de test actuel et le réseau principal. Assurez-vous d'initialiser la variable tezosNode en conséquence. L'interface de cette fonctionnalité se trouve dans les espaces de noms TezosNodeReaderet TezosNodeWriter. Ces opérations ne nécessitent pas de clé API. ConseilJS peut être utilisé avec n'importe quel nœud Tezos compatible. «Compatible» dans ce cas signifie exécuter une version de protocole spécifique. Au moment de la rédaction de cet article, le protocole du réseau principal était P006, Carthage qui est pris en charge par ConseilJS 5.0.3 et versions ultérieures.
#### **[Créer un compte Tezos testnet](https://cryptonomic.github.io/ConseilJS/#/?id=create-a-tezos-testnet-account)**
Visitez le [robinet](https://faucet.tzalpha.net/) pour obtenir un compte de test avec un équilibre sain de faux XTZ avec lesquels jouer. Avec ces informations, nous pouvons créer les clés publiques et privées nécessaires pour participer à l'un des réseaux de test. Cryptonomic permet d'accéder au réseau principal de Tezos et aux réseaux carthagenet (à partir du premier trimestre 2020) via [Nautilus Cloud](https://nautilus.cloud/) . Dans les exemples ci-dessous, le contenu du fichier produit par le robinet est affecté à faucetAccount. Notez que celui le contenu de faucetAccount utilisé dans ces exemples ne fonctionnera certainement pas, veuillez réclamer le vôtre au [robinet](https://faucet.tzalpha.net/).

    import { KeyStoreUtils } from 'conseiljs-softsigner';
    const faucetAccount = {
    "mnemonic": [ "letter", "pair", "shuffle", "exotic", "sword", "west", "build", "monster", "future", "senior", "salt", "satisfy", "knock", "alert", "gorilla"],
    "secret": "0a09075426ab2658814c1faf101f53e5209a62f5",
    "amount": "5652123072",
    "pkh": "tz1QSHaKpTFhgHLbqinyYRjxD5sLcbfbzhxy",
    "password": "VvLbJMu1fF",
    "email": "yoyhmapi.ugewcsiv@tezos.example.org"
    }
    async function initAccount() {
    const keyStore = await KeyStoreUtils.restoreIdentityFromFundraiser(faucetAccount.mnemonic.join(' '), faucetAccount.email, faucetAccount.password, faucetAccount.pkh);
    console.log(`public key: ${keystore.publicKey}`);
    console.log(`secret key: ${keystore.secretKey}`);
    }
    initAccount();
