ConseilJS
Une bibliothèque pour construire des applications décentralisées en Typescript et Javascript, actuellement centrée sur la plateforme Tezos .
ConseilJS se connecte aux nœuds Tezos pour les données et les opérations de la chaîne en direct et aux serveurs Conseil pour des analyses haute performance sur les données de la blockchain. En interne, Cryptonomic utilise Nautilus pour les déploiements d'infrastructure de ces services. C'est la bibliothèque au cœur de nos produits, Arronax , Mininax et certainement Galleon . Il existe également des liaisons ReasonML .
Cryptonomic propose un service d'infrastructure - Nautilus Cloud qui permet un accès rapide à la plate-forme Tezos avec des produits qui facilitent sa construction.
Autres ressources
En plus de ces documents, il existe une procédure automatisée pour les flux de travail Tezos courants . L' explorateur de blocs Mininax est pratique pour visualiser les opérations soumises pendant cette exécution. Cryptonomic organise régulièrement des ateliers de développement d’application à l'échelle internationale, si vous êtes intéressé par un, contactez-le pour l'organiser. Le matériel pour ces événements est continuellement mis à jour et est disponible sur Medium .
Utiliser avec Nodejs
Ajoutez notre package NPM à votre projet et une bibliothèque de signature. Depuis ConseilJS 5.0.0, la fonctionnalité de signature cryptographique est externe à la bibliothèque principale. De plus, il est nécessaire de fournir un enregistreur et de récupérer les références comme spécifié ci-dessous. Ceci est fait séparément pour permettre l'utilisation de la récupération prenant en charge le proxy par exemple.
commandes bash
npm i conseiljs
npm i conseiljs-softsigner
 
 
code javascript
import fetch from 'node-fetch';
import * as log from 'loglevel';
 
import { registerFetch, registerLogger, Signer, TezosMessageUtils } from 'conseiljs';
import { KeyStoreUtils, SoftSigner } from 'conseiljs-softsigner';
 
const logger = log.getLogger('conseiljs');
logger.setLevel('debug', false); // to see only errors, set to 'error'
registerLogger(logger);
registerFetch(fetch);
 
let signer: Signer;
const keyStore = await KeyStoreUtils.restoreIdentityFromSecretKey('edskRgu8wHxjwayvnmpLDDijzD3VZDoAH7ZLqJWuG4zg7LbxmSWZWhtkSyM5Uby41rGfsBGk4iPKWHSDniFyCRv3j7YFCknyHH');
signer = await SoftSigner.createSigner(TezosMessageUtils.writeKeyWithHint(keyStore.secretKey, 'edsk'), -1);
 
 
 
 
 
 
 
Utiliser avec le Web
Contrairement à l'exemple nodejs, il n'est pas possible de configurer des références de récupération ou de journalisation pour la version Web de ConseilJS.
code HTML
<html>
<head>
    <script src="https://cdn.jsdelivr.net/gh/cryptonomic/conseiljs/dist-web/conseiljs.min.js"
        integrity="sha384-IufW5flMmPmFPiUm4GlKLocLuEWZlGoCA5ukejQgI66LcXELSsraBP7dux+BE9Ds"
        crossorigin="anonymous"></script>
    <script src="https://cdn.jsdelivr.net/gh/cryptonomic/conseiljs-softsigner/dist-web/conseiljs-softsigner.min.js"
        crossorigin="anonymous"></script>
    <script>
        const keyStore = await conseiljssoftsigner.KeyStoreUtils.restoreIdentityFromSecretKey('edskRgu8wHxjwayvnmpLDDijzD3VZDoAH7ZLqJWuG4zg7LbxmSWZWhtkSyM5Uby41rGfsBGk4iPKWHSDniFyCRv3j7YFCknyHH');
        signer = await conseiljssoftsigner.SoftSigner.createSigner(TezosMessageUtils.writeKeyWithHint(keyStore.secretKey, 'edsk'), -1);
    </script>
</head>
<body>
    ...
</body>
</html>
Si vous utilisez le mécanisme de vérification d'intégrité, assurez-vous de garder le hachage à jour. La référence ci-dessus indique le tronc ConseilJS sur GitHub et il est mis à jour lorsque de nouvelles versions sont effectuées, ce qui rendra les hachages précédents invalides et rendra votre page Web non fonctionnelle. Pour cette raison, les exemples de code suivants n'incluent pas l'attribut integrity.
Présentation et exemples de l'API
Développement de contrat Lightning Route
Si vous souhaitez passer directement au travail sur les contrats intelligents Michelson, suivez simplement ces instructions dans l'ordre suivant:
Créer un projet de node
Install ConseilJS
Obtenez une clé API
Créer un compte Tezos testnet
Initialiser le compte
Déployer un contrat
Invoquer un contrat
Blockchain Analytics Lightning Route
Si vous vous intéressez à la statistique et à l'intelligence d'affaires, ConseilJS dispose également d'outils pour cela. Alors que pour une extraction de données plus lourde, l'utilisation directe de Conseil peut avoir plus de sens, mais si vous créez des outils utilisateur tels que des tableaux de bord, ConseilJS facilitera considérablement le processus.
Créer un projet de nœud
Install ConseilJS
Obtenez une clé API
Exécuter des rapports
clé API
Certaines fonctions ConseilJS nécessitent une clé API pour une instance de service Conseil. Alors que les interactions directes en chaîne se produisent via un nœud Tezos, les opérations de lecture de Conseil comme celles de l' TezosConseilClientespace de noms nécessitent une clé API.
L'obtention d'une clé de développement est facile. Cryptonomic propose un service d'infrastructure - Nautilus Cloud qui permet un accès rapide à la plate-forme Tezos avec des produits qui facilitent sa construction.
Opérations de la chaîne Tezos
Pour exécuter des opérations sur la chaîne Tezos, un lien vers un nœud Tezos est nécessaire. Un lien  peut être trouvé après la connexion à Nautilus Cloud pour le réseau de test actuel et le réseau principal. Assurez-vous d'initialiser la variable tezosNode en conséquence. L'interface de cette fonctionnalité se trouve dans les espaces de noms TezosNodeReaderet TezosNodeWriter. Ces opérations ne nécessitent pas de clé API. ConseilJS peut être utilisé avec n'importe quel nœud Tezos compatible. «Compatible» dans ce cas signifie exécuter une version de protocole spécifique. Au moment de la rédaction de cet article, le protocole du réseau principal était P006, Carthage qui est pris en charge par ConseilJS 5.0.3 et versions ultérieures.
Créer un compte Tezos testnet
Visitez le robinet pour obtenir un compte de test avec un équilibre sain de faux XTZ avec lesquels jouer. Avec ces informations, nous pouvons créer les clés publiques et privées nécessaires pour participer à l'un des réseaux de test. Cryptonomic permet d'accéder au réseau principal de Tezos et aux réseaux carthagenet (à partir du premier trimestre 2020) via Nautilus Cloud . Dans les exemples ci-dessous, le contenu du fichier produit par le robinet  est affecté à  faucetAccount. Notez que celui le contenu de  faucetAccount utilisé dans ces exemples ne fonctionnera certainement pas, veuillez réclamer le vôtre au robinet.
 
 
 
 
 
 
 
 
 
 
typescript code source
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
 

 
 
 


javascript source code

const conseiljssoftsigner = require('conseiljs-softsigner');

const faucetAccount = {
    "mnemonic": [ "letter", "pair", "shuffle", "exotic", "sword", "west", "build", "monster", "future", "senior", "salt", "satisfy", "knock", "alert", "gorilla"],
    "secret": "0a09075426ab2658814c1faf101f53e5209a62f5",
    "amount": "5652123072",
    "pkh": "tz1QSHaKpTFhgHLbqinyYRjxD5sLcbfbzhxy",
    "password": "VvLbJMu1fF",
    "email": "yoyhmapi.ugewcsiv@tezos.example.org"
}

async function initAccount() {
    const keyStore = await conseiljssoftsigner.KeyStoreUtils.restoreIdentityFromFundraiser(faucetAccount.mnemonic.join(' '), faucetAccount.email, faucetAccount.password, faucetAccount.pkh);
    console.log(`public key: ${keystore.publicKey}`);
    console.log(`secret key: ${keystore.secretKey}`);
}

initAccount();


les bout de code ci dessus permettent de produire une clé publique edpkvQtuhdZQmjdjVfaY9Kf4hHfrRJYugaJErkCGvV3ER1S7XWsrrj et une clé secrète de edskRgu8wHxjwayvnmpLDDijzD3VZDoAH7ZLqJWuG4zg7LbxmSWZWhtkSyM5Uby41rGfsBGk4iPKWHSDniFyCRv3j7YFCknyHH. Il est important de noter que  les clés secrètes doivent être gardées en sécurité !

Créer un compte Tezos vide

Il est également possible de créer des comptes indépendants sur la chaîne. Le processus est identique à celui utilisé  pour créer des comptes sur le réseau principal qui ne sont pas des comptes de collecte de fonds. Notez que restoreIdentityFromMnemonic prend un mot de passe facultatif. Ce n'est pas le même mot de passe qui peut avoir été utilisé pour crypter le KeyStoreUtils fichier. La présentation du mnémonique intermédiaire à l'utilisateur peut faciliter la récupération des clés.






code source typescript

import { KeyStoreUtils } from 'conseiljs-softsigner';

async function createAccount() {
    const mnemonic = KeyStoreUtils.generateMnemonic();
    console.log(`mnemonic: ${mnemonic}`);
    const keystore = await KeyStoreUtils.restoreIdentityFromMnemonic(mnemonic, '');
    console.log(`account id: ${keystore.publicKeyHash}`);
    console.log(`public key: ${keystore.publicKey}`);
    console.log(`secret key: ${keystore.secretKey}`);
}

createAccount();

code source javascript

const conseiljssoftsigner = require('conseiljs-softsigner');

async function createAccount() {
    const mnemonic = conseiljssoftsigner.KeyStoreUtils.generateMnemonic();
    console.log(`mnemonic: ${mnemonic}`);
    const keystore = await conseiljssoftsigner.KeyStoreUtils.unlockIdentityWithMnemonic(mnemonic, '');
    console.log(`account id: ${keystore.publicKeyHash}`);
    console.log(`public key: ${keystore.publicKey}`);
    console.log(`secret key: ${keystore.secretKey}`);
}

createAccount();


Vous pouvez aussi utiliser KeyStoreUtils.generateIdentity()



Initialiser le compte

Un robinet (ou compte indépendant) ou un compte de collecte de fonds doit être activé sur la chaîne avant de pouvoir être utilisé. Avec la combinaison de la sortie du robinet et des clés générées à l'étape ci-dessus, nous pouvons envoyer une opération d'activation de compte. Il n'est ni possible ni nécessaire d'activer un compte non associé, cette opération échouera tout simplement pour ceux-ci.





code source Typescript

import { TezosNodeWriter, KeyStoreType } from 'conseiljs';
import { SoftSigner } from 'conseiljs-softsigner';

const tezosNode = ''; // get access as https://nautilus.cloud

async function activateAccount() {
    const keystore = {
        publicKey: 'edpkvQtuhdZQmjdjVfaY9Kf4hHfrRJYugaJErkCGvV3ER1S7XWsrrj',
        secretKey: 'edskRgu8wHxjwayvnmpLDDijzD3VZDoAH7ZLqJWuG4zg7LbxmSWZWhtkSyM5Uby41rGfsBGk4iPKWHSDniFyCRv3j7YFCknyHH',
        publicKeyHash: 'tz1QSHaKpTFhgHLbqinyYRjxD5sLcbfbzhxy',
        seed: '',
        storeType: KeyStoreType.Fundraiser
    };

    const signer = await SoftSigner.createSigner(TezosMessageUtils.writeKeyWithHint(keyStore.secretKey, 'edsk'));
    const result = await TezosNodeWriter.sendIdentityActivationOperation(tezosNode, signer, keystore, '0a09075426ab2658814c1faf101f53e5209a62f5');
    console.log(`Injected operation group id ${result.operationGroupID}`)
}

activateAccount();


code source javascript

const conseiljs = require('conseiljs');
const conseiljssoftsigner = require('conseiljs-softsigner');
const tezosNode = ''; // get access as https://nautilus.cloud

async function activateAccount() {
    const keystore = {
        publicKey: 'edpkvQtuhdZQmjdjVfaY9Kf4hHfrRJYugaJErkCGvV3ER1S7XWsrrj',
        secretKey: 'edskRgu8wHxjwayvnmpLDDijzD3VZDoAH7ZLqJWuG4zg7LbxmSWZWhtkSyM5Uby41rGfsBGk4iPKWHSDniFyCRv3j7YFCknyHH',
        publicKeyHash: 'tz1QSHaKpTFhgHLbqinyYRjxD5sLcbfbzhxy',
        seed: '',
        storeType: conseiljs.KeyStoreType.Fundraiser
    };

    const signer = await conseiljssoftsigner.SoftSigner.createSigner(conseiljs.TezosMessageUtils.writeKeyWithHint(keyStore.secretKey, 'edsk'));
    const result = await conseiljs.TezosNodeWriter.sendIdentityActivationOperation(tezosNode, keystore, '0a09075426ab2658814c1faf101f53e5209a62f5');
    console.log(`Injected operation group id ${result.operationGroupID}`)
}

activateAccount();

code source activation sur le Web

<html>
<head>
    <script src="https://cdn.jsdelivr.net/gh/cryptonomic/conseiljs/dist-web/conseiljs.min.js"
        crossorigin="anonymous"></script>
    <script src="https://cdn.jsdelivr.net/gh/cryptonomic/conseiljs-softsigner/dist-web/conseiljs-softsigner.min.js"
        crossorigin="anonymous"></script>
    <script>
        const tezosNode = ''; // get access as https://nautilus.cloud

        async function activateAccount() {
            const keystore = {
                publicKey: 'edpkvQtuhdZQmjdjVfaY9Kf4hHfrRJYugaJErkCGvV3ER1S7XWsrrj',
                secretKey: 'edskRgu8wHxjwayvnmpLDDijzD3VZDoAH7ZLqJWuG4zg7LbxmSWZWhtkSyM5Uby41rGfsBGk4iPKWHSDniFyCRv3j7YFCknyHH',
                publicKeyHash: 'tz1QSHaKpTFhgHLbqinyYRjxD5sLcbfbzhxy',
                seed: '',
                storeType: conseiljs.KeyStoreType.Fundraiser
            };

            const signer = await conseiljssoftsigner.SoftSigner.createSigner(conseiljs.TezosMessageUtils.writeKeyWithHint(keyStore.secretKey, 'edsk'));
            const result = await conseiljs.TezosNodeWriter.sendIdentityActivationOperation(tezosNode, keystore, '0a09075426ab2658814c1faf101f53e5209a62f5');
            console.log(`Injected operation group id ${result.operationGroupID}`)
        }

        activateAccount();
    </script>
</head>
<body>
<form>
    <button onClick="activateAccount(); return false;">Activate Account</button>
</form>
</body>
</html>


La prochaine étape sur Tezos après l’activation du compte  est une révélation de celui-ci. La révélation  est requise pour tous les comptes. Cette opération enregistre la clé publique du compte sur la chaîne. La révélation de compte est une opération payante, il n'est pas possible de révéler un compte à solde 0 nouvellement créé.

code source Typescript pour la révélation d’un compte 

import { TezosNodeWriter, KeyStoreType } from 'conseiljs';
import { SoftSigner } from 'conseiljs-softsigner';

const tezosNode = '';

async function revealAccount() {
    const keystore = {
        publicKey: 'edpkvQtuhdZQmjdjVfaY9Kf4hHfrRJYugaJErkCGvV3ER1S7XWsrrj',
        secretKey: 'edskRgu8wHxjwayvnmpLDDijzD3VZDoAH7ZLqJWuG4zg7LbxmSWZWhtkSyM5Uby41rGfsBGk4iPKWHSDniFyCRv3j7YFCknyHH',
        publicKeyHash: 'tz1QSHaKpTFhgHLbqinyYRjxD5sLcbfbzhxy',
        seed: '',
        storeType: KeyStoreType.Fundraiser
    };

    const signer = await SoftSigner.createSigner(TezosMessageUtils.writeKeyWithHint(keyStore.secretKey, 'edsk'));
    const result = await TezosNodeWriter.sendKeyRevealOperation(tezosNode, signer, keystore);
    console.log(`Injected operation group id ${result.operationGroupID}`);
}

revealAccount();




















code source Javascript pour la révélation d’un compte 

const conseiljs = require('conseiljs');
const conseiljssoftsigner = require('conseiljs-softsigner');
const tezosNode = '';

async function revealAccount() {
    const keystore = {
        publicKey: 'edpkvQtuhdZQmjdjVfaY9Kf4hHfrRJYugaJErkCGvV3ER1S7XWsrrj',
        secretKey: 'edskRgu8wHxjwayvnmpLDDijzD3VZDoAH7ZLqJWuG4zg7LbxmSWZWhtkSyM5Uby41rGfsBGk4iPKWHSDniFyCRv3j7YFCknyHH',
        publicKeyHash: 'tz1QSHaKpTFhgHLbqinyYRjxD5sLcbfbzhxy',
        seed: '',
        storeType: conseiljs.KeyStoreType.Fundraiser
    };

    const signer = await conseiljssoftsigner.SoftSigner.createSigner(conseiljs.TezosMessageUtils.writeKeyWithHint(keyStore.secretKey, 'edsk'));
    const result = await conseiljs.TezosNodeWriter.sendKeyRevealOperation(tezosNode, signer, keystore);
    console.log(`Injected operation group id ${result.operationGroupID}`);
}

revealAccount();




Transfert de valeur

L'opération la plus élémentaire de la chaîne est le transfert de valeur entre deux comptes. Dans cet exemple,  nous avons le compte activé ci - dessus: tz1QSHaKpTFhgHLbqinyYRjxD5sLcbfbzhxy et une adresse testnet aléatoire test avec: tz1RVcUP9nUurgEJMDou8eW3bVDs6qmP5Lnc. Notez que toutes les quantités sont en µtz, comme en micro-tez, donc 0,5 tz est représenté par 500000. Les frais de 1500 ont  été choisis arbitrairement, mais certaines opérations ont des exigences de frais minimum.










code source TypeScript de transfert de valeur

import { TezosNodeWriter, KeyStoreType } from 'conseiljs';
import { SoftSigner } from 'conseiljs-softsigner';

const tezosNode = '';

async function sendTransaction() {
    const keystore = {
        publicKey: 'edpkvQtuhdZQmjdjVfaY9Kf4hHfrRJYugaJErkCGvV3ER1S7XWsrrj',
        secretKey: 'edskRgu8wHxjwayvnmpLDDijzD3VZDoAH7ZLqJWuG4zg7LbxmSWZWhtkSyM5Uby41rGfsBGk4iPKWHSDniFyCRv3j7YFCknyHH',
        publicKeyHash: 'tz1QSHaKpTFhgHLbqinyYRjxD5sLcbfbzhxy',
        seed: '',
        storeType: KeyStoreType.Fundraiser
    };

    const signer = await SoftSigner.createSigner(TezosMessageUtils.writeKeyWithHint(keyStore.secretKey, 'edsk'));
    const result = await TezosNodeWriter.sendTransactionOperation(tezosNode, signer, keystore, 'tz1RVcUP9nUurgEJMDou8eW3bVDs6qmP5Lnc', 500_000, 1500);
    console.log(`Injected operation group id ${result.operationGroupID}`);
}

sendTransaction();






















code source Javascript de transfert de valeur

const conseiljs = require('conseiljs');
const conseiljssoftsigner = require('conseiljs-softsigner');
const tezosNode = '';

async function sendTransaction() {
    const keystore = {
        publicKey: 'edpkvQtuhdZQmjdjVfaY9Kf4hHfrRJYugaJErkCGvV3ER1S7XWsrrj',
        secretKey: 'edskRgu8wHxjwayvnmpLDDijzD3VZDoAH7ZLqJWuG4zg7LbxmSWZWhtkSyM5Uby41rGfsBGk4iPKWHSDniFyCRv3j7YFCknyHH',
        publicKeyHash: 'tz1QSHaKpTFhgHLbqinyYRjxD5sLcbfbzhxy',
        seed: '',
        storeType: conseiljs.KeyStoreType.Fundraiser
    };

    const signer = await conseiljssoftsigner.SoftSigner.createSigner(conseiljs.TezosMessageUtils.writeKeyWithHint(keyStore.secretKey, 'edsk'));
    const result = await conseiljs.TezosNodeWriter.sendTransactionOperation(tezosNode, signer, keystore, 'tz1RVcUP9nUurgEJMDou8eW3bVDs6qmP5Lnc', 500000, 1500);
    console.log(`Injected operation group id ${result.operationGroupID}`);
}

sendTransaction();




 
 
 
 
 
 
 
 
 
 
 
Déléguer
L'une des fonctionnalités les plus intéressantes de Tezos est la délégation. C'est un moyen pour les comptes non "boulangers" (non validateurs) de participer au processus de gouvernance en chaîne et de recevoir des récompenses de jalonnement. Il est possible de déléguer à la fois des comptes implicites et créés (comptes d'origine). Pour les adresses implicites, celles commençant par tz1, tz2 et tz3, appelez simplement sendDelegationOperation. Les comptes d'origine, c'est-à-dire les contrats intelligents, doivent explicitement prendre en charge l'attribution de délégué, mais peuvent également être déployés avec un délégué déjà défini.
Notez que l'appel sendDelegationOperation modifiera une délégation existante si elle est définie. Pour annuler l'utilisation de la délégation sendUndelegationOperation. La définition de l'adresse de délégation sur les libellés  de votre propres  compte en tant que validateur et doit être évitée sauf si elle est intentionnelle. L'envoi d'une délégation à la même adresse que celle déjà définie entraînera un échec de l'opération.
Un autre point important est que la délégation est appliquée par compte pour le solde total de ce compte. Pour déléguer à plusieurs boulangers à partir d'une seule adresse, utilisez BabylonDelegationHelper.
 
 
 
 
 
 
 
 
 
 
 
code source delegation en Typescript
import { TezosMessageUtils, TezosNodeWriter, KeyStoreType } from 'conseiljs';
import { SoftSigner } from 'conseiljs-softsigner';
 
const tezosNode = '';
 
async function delegateAccount() {
    const keystore = {
        publicKey: 'edpkvQtuhdZQmjdjVfaY9Kf4hHfrRJYugaJErkCGvV3ER1S7XWsrrj',
        secretKey: 'edskRgu8wHxjwayvnmpLDDijzD3VZDoAH7ZLqJWuG4zg7LbxmSWZWhtkSyM5Uby41rGfsBGk4iPKWHSDniFyCRv3j7YFCknyHH',
        publicKeyHash: 'tz1QSHaKpTFhgHLbqinyYRjxD5sLcbfbzhxy',
        seed: '',
        storeType: KeyStoreType.Fundraiser
    };
 
    const signer = await SoftSigner.createSigner(TezosMessageUtils.writeKeyWithHint(keyStore.secretKey, 'edsk'));
    const result = await TezosNodeWriter.sendDelegationOperation(tezosNode, signer, keystore, 'tz1VxS7ff4YnZRs8b4mMP4WaMVpoQjuo1rjf', 10000);
    console.log(`Injected operation group id ${result.operationGroupID}`);
}
 
delegateAccount();
 
 
code source delegation en Javascript
const conseiljs = require('conseiljs');
const conseiljssoftsigner = require('conseiljs-softsigner');
const tezosNode = '';
 
async function delegateAccount() {
    const keystore = {
        publicKey: 'edpkvQtuhdZQmjdjVfaY9Kf4hHfrRJYugaJErkCGvV3ER1S7XWsrrj',
        secretKey: 'edskRgu8wHxjwayvnmpLDDijzD3VZDoAH7ZLqJWuG4zg7LbxmSWZWhtkSyM5Uby41rGfsBGk4iPKWHSDniFyCRv3j7YFCknyHH',
        publicKeyHash: 'tz1QSHaKpTFhgHLbqinyYRjxD5sLcbfbzhxy',
        seed: '',
        storeType: conseiljs.KeyStoreType.Fundraiser
    };
 
    const signer = await conseiljssoftsigner.SoftSigner.createSigner(conseiljs.TezosMessageUtils.writeKeyWithHint(keyStore.secretKey, 'edsk'));
    const result = await conseiljs.TezosNodeWriter.sendDelegationOperation(tezosNode, signer, keystore, 'tz1VxS7ff4YnZRs8b4mMP4WaMVpoQjuo1rjf', 10000);
    console.log(`Injected operation group id ${result.operationGroupID}`);
}
 
delegateAccount();
 
 
Interactions de contrats intelligents
Les contrats intelligents Tezos sont exécutés nativement dans un langage de type sécurisé basé sur la pile appelé Michelson. ConseilJS est en mesure de déployer une grande partie des contrats rédigés dans ceux langage (Michelson). Plutôt que de rédiger des contrats directement dans Michelson, nous vous encourageons à envisager des options plus conviviales pour les développeurs, telles que SmartPy . Smart Chain Arena a du matériel d'apprentissage et une excellente référence pour SmartPy . L'éditeur dispose également d'un ensemble d'exemples de contrats qui illustrent diverses techniques de contrat intelligent. Enfin, Cryptonomic a mis en place un Syllabus de développement de contrat intelligent basé sur SmartPy.
 
Déployer un contrat
 


























