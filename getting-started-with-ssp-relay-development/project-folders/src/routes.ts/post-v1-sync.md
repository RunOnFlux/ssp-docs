---
description: Sync using data specified in the request body
---

# POST /v1/sync

<mark style="color:green;">`POST`</mark> **/v1/sync**

**Headers**

| Name         | Value            |
| ------------ | ---------------- |
| Content-Type | application/json |

**Body**

| Parameter        | Description                          |
| ---------------- | ------------------------------------ |
| chain            | chain where the id is associated     |
| walletIdentity   | id being used to identify the wallet |
| keyXpub          | xpub key of the wallet               |
| wkIdentity       | wallet key value                     |
| publicNonces     | public nonces value                  |
| generatedAddress | address associated with the wallet   |

```javascript
{
    "chain": "btc",
    "walletIdentity": "1Kys4uBjfqcb3DrtkAs8Gc8YXEmDmeFESd",
    "keyXpub": "Zpub75iXLooqV4JZNAC68gK6kAdNmAdY2wMMUeGxv9rZHHnWhU91EccH6r45CmYUnADR51p4H3aFkWLicnV5wj69GipUgA5eNM8RnYAtNpAxWb5",
    "wkIdentity": "bc1q4q6nhujzlpll9e2mkjlqddmdfyvh4a89vqatsk0ju8jwt2vghf9sykaw0c",
    "publicNonces": [
        {
            "kPublic": "027fa9f9c1e8cbd77799d39be7e06ea55dbe793fd1b74857e3a83e7de866e07027",
            "kTwoPublic": "037b87c588195c9883ca18b9e5c8acb156bf9ff4193dc6db381361e2ca2c386046"
        },
        {
            "kPublic": "03d1c7179c86752f6f0c24d726c43e42ef902461fb1ad805ffe06cae5a8ac05814",
            "kTwoPublic": "02ab3045c751591bb7ec2a236d9e42794f1692788f0f11f97164e3b02e75fd8d0d"
        },
        {
            "kPublic": "02c512ddf6fd8f5927adabb2c9ca38c22c6355bec6e27d5a3e01ca608eb4e40ce0",
            "kTwoPublic": "026b831445a028ac460fed5fb3579562c69ebdf9465414ec7ba7eb0a2bb8471840"
        },
        {
            "kPublic": "03c764f7c7f10e9935da59824954dcb1cd3870bfd7dd6b46b27e9b9b75fb51df90",
            "kTwoPublic": "03b5f5a9cd2cdfe7ea3621dc6b02fedb7a04093f9b32bf006577d6395360b583b9"
        },
        {
            "kPublic": "02ba82c7f0dec94fe54e47bd384554729a6a817d2ff875bfc11ff72e174453ca3b",
            "kTwoPublic": "034b84869ab98addafcb29bd0463741c03dcdf361d356bba3bf7bf8e666cbe6b54"
        },
        {
            "kPublic": "026fe4afabaae005ce1c9ba58b19b4a7e9f6934334bdecd5e10897a05b3015b569",
            "kTwoPublic": "020b6924a24966b27bdec49272dca1af5209387d9466f419c4ff57d63c1f6e23b2"
        },
        {
            "kPublic": "036ac536fdbfbdd3ab4b53b7fdbfb887e548f97fdeeeb68e8babe4fa44eef7a13f",
            "kTwoPublic": "02702b3d406b5eefc0183e7d2c734676595ff87f36c3abf41e23d30d9ce0c77f01"
        },
        {
            "kPublic": "0297c648c8aef9162bcb381e4e6fb3c80747d48965b974635923d36724fe52f77e",
            "kTwoPublic": "0227696fa75d105be1b799becdb55cdb94cd29f8b39a7f7fcd15a69a5fe6b28874"
        },
        {
            "kPublic": "022a4ab9d2c942784eb9fd0b57d367a55296b96ebb5c03d5da86aad5c18258e9f0",
            "kTwoPublic": "020f11d46c8ebb0a16e7af8b42b6f8ffa728e900f6f15e5d1f1ec5eb0ffb77ab7f"
        },
        {
            "kPublic": "034448b2a43053b493a6ede09adda28c5036e4c5b5cfd493dda1567fc8b2a0676f",
            "kTwoPublic": "02ff4f296aa4108ed317bd67c12c41a0a1cea8ca1477e8c93b0dfdbefb9c849668"
        },
        {
            "kPublic": "03eb6487f0984c629c4de0cec57b4cbfdfc7d411a068a47906984a9d71d2e3d0ca",
            "kTwoPublic": "03da21ac3cc833d7e5ad38b016f020455cf9ec09119f26d7be7fdcc122399ca841"
        },
        {
            "kPublic": "032ee70475b5a6e082fe0765eaed566e35f5f2169e7a9483bd6a82b0da7291a442",
            "kTwoPublic": "03133fa2a3731c2d1ffb3a51ce68405fa7059da0da8b9130a6c92ee7a443a20c99"
        },
        {
            "kPublic": "03145d07decc92e8119f2e6853e0ec09adb3f0b85e40aecc77e9e66d0a5ae92169",
            "kTwoPublic": "025791e4d08f500c3c468a4603e4409c45f4c4be5ffd84d859ec9798f38496d25c"
        },
        {
            "kPublic": "0331cc678b16484f6d4597e21b2b5a92501ae1691068abef9fb2e390c5b4aefa07",
            "kTwoPublic": "03c13c2eb0d7be04532913d758fb560348d5dc14e009d5b380d7e6a2194e288e2f"
        },
        {
            "kPublic": "02c35f88926be156aa7178061e6103925cd7b11796557d19a53e92bd9f667c195f",
            "kTwoPublic": "0353a213b38556ee95eb6fc72b137660c392b07e42f8cd802d626556d763d7313e"
        },
        {
            "kPublic": "03f38bd700f7c0a833d328d7adf33ea1c33752d95fc3794bbe3110f5ebb4cd7a14",
            "kTwoPublic": "03c7768aa2901b046bbee907691de2bf36ee9423d6410f84788027591e1d8f4edf"
        },
        {
            "kPublic": "02a9758e0d8741e1732e9c123e8c67634bfef7e52c9edd8befffd4e456707d1836",
            "kTwoPublic": "021d5022d12a5a20318ddad8827af1c6e2308c4710600fad63b85b2021224d35ea"
        },
        {
            "kPublic": "02bffebd506b888c62ade28245d9ebb3c8353d48baf749067f37973770a15888e3",
            "kTwoPublic": "02ca23f34e0443565c3c91627b30a0e4fdb3c99d3477efea354f47a22e6900ee75"
        },
        {
            "kPublic": "031d1963e434ce049e0c82199103b0ee5cf1f6f04437dac33956041d04e73446af",
            "kTwoPublic": "032fad849c38b555883c341ea1b0362f5135a75944bd0a15529825b0b8457988d6"
        },
        {
            "kPublic": "039193ee2267106322acb712950700db43850be7fc3c297ce5d51033d3d1da72f5",
            "kTwoPublic": "0317da113a69377ae1807050becb80325e71146c9133a32820bdf56cb7b64ba5ab"
        },
        {
            "kPublic": "032f085acb7109ef7a772d96367cf34ffda6e485aaced64cffc19cca57455472f8",
            "kTwoPublic": "02b2eb3c82361fee262f61a887d900daaa0fa03b369e14551a972c7db0efab5149"
        },
        {
            "kPublic": "03e08233a625f1086836d4274398a16b471e187cea3d3eb841b3f8e777e6f385b1",
            "kTwoPublic": "02aa9322e7b4f8bbb0aa6e7527b07761aff79b6c7e9b471ba64d49e1e2d0cfb1ad"
        },
        {
            "kPublic": "03a6801c45aa35be7dbf630fbb16d4150433320ada2af73016b821a22a3ffdf351",
            "kTwoPublic": "03331f7260f679b90f63211b46b7938322a37cc6897020a03802a9f4b976fb71bf"
        },
        {
            "kPublic": "032b01f558f808cc6b59f5048b1ab9fa91889e9e5d01c17cd24cfa679784e9bc18",
            "kTwoPublic": "029de37a609ba223ade79eae7422571f73305e739f6c3895dfefa5d1bb10addd38"
        },
        {
            "kPublic": "03ebf9f9cd36f591a3dc6875ae673ef9731610326d72f9b9286cc43c45ab24293e",
            "kTwoPublic": "0245400480b22a156194399db6a367068df8e56c7f0033dec53618b9fd2ca94796"
        },
        {
            "kPublic": "03eaa11310bb09f65abf897ed2fc73d3b4f66e3f7b807ef3d73cbe0f54ce686c7a",
            "kTwoPublic": "031c151da1e78ba5620020a73f5b1dd9311a00b95d9c8ef7950ffe4199008c26f3"
        },
        {
            "kPublic": "03165c9d432229b2dcfd580d618f210daf2e7ef44dda087788edcb2e378a7b89e3",
            "kTwoPublic": "03c8a5e37e3488b00282d2b13caff7f91e0915d076cce1e2f0d2d8a03b91ff6dde"
        },
        {
            "kPublic": "02f7a04407cad6860e8d1f804a38f0da380f84be806ff415fedb9c533656ca9207",
            "kTwoPublic": "02c80dde44469e9c794ec5708edb6d05262d62dec906be8a0d146adc3ba4e05448"
        },
        {
            "kPublic": "03f4d043c18c1f1842074f9859d82e0000d3f2d7bdc1f75a2a3e185950be945659",
            "kTwoPublic": "02185c61b039a55c8a56aca5e25bd2cc8e3f3761042da9ce245aeab5d4e45b1020"
        },
        {
            "kPublic": "02bf52bec05bc3b344905410e7ac230d19ceabd49ec9e793c18db6d8d51d8c922b",
            "kTwoPublic": "0331952f8b5feecae6543b514cb1929e55f477463dbaf3e90cac695e481dcbf3e0"
        },
        {
            "kPublic": "02f2d5bfdfd231a8c82f391e7cb975572de8cfdfcef8028affa7843225656ea6ac",
            "kTwoPublic": "0283e0356a437f6218f865a409632737364d1fa7eef1b391d83ba09713ad39748e"
        },
        {
            "kPublic": "0209b403e2f7e3565d09dd32aa7ba389456839b28bfcaa1d93aa3f6b52b509339e",
            "kTwoPublic": "02966078a7e3bf88af05cec889268dbb4510d63193bc4db8cfc516a3dd3e77577a"
        },
        {
            "kPublic": "025c4aa8a7b1e88a10a0983df1269cf9908b0bc15a3b1d32e4888eeb057c7e01a9",
            "kTwoPublic": "02d8e80845d57e340bd6ed1eb3b9192878cc220b6e4c60e2ee36ded22a36bdbbbc"
        },
        {
            "kPublic": "02022b9806d8f952225a48a2de4ceedbe4a1d2b4b57448153d9c564e1a30251f6a",
            "kTwoPublic": "02007a93caec5a514dce9b14b9f26774e2c790af398cd4561e0790a200c005db10"
        },
        {
            "kPublic": "02702968776d33e53adb722a41291b166984f3fd65c5e0335f75d8c98f13289ad3",
            "kTwoPublic": "02658ce5fa3c905a953490e8c2ef567343525446c8c60e4d115ac07ff867b248d6"
        },
        {
            "kPublic": "0364f0fb53a5dcd03bb4110a5d80bfbb5c7b853bcdb31a3fb49cc8b8dbc658795a",
            "kTwoPublic": "03bfbb44069fc3e55b304488869927a6b76d7db9fc58ac9094039f50e49b679962"
        },
        {
            "kPublic": "0343dc67ce850f932c5cbddb6dafa42899fc89a87c130f66397807cf7181a01b9c",
            "kTwoPublic": "02470145d4b50c477a06e5b06baa5f58f5b006dfd68c98923828db485261ef1132"
        },
        {
            "kPublic": "02ee9d8e52ad05461fad7045d58d93af849ab9fe7847bbc2bc22384e896697f871",
            "kTwoPublic": "02a11c7d1d624b18d2e24afdd6303f984a0f0208a294dc93d8bbda66f72c03b88c"
        },
        {
            "kPublic": "0266d99ff948390ce7bf789a4813848af0e96ce81a6342f7048bcf0bea1fbf6e07",
            "kTwoPublic": "02a960aa9509f41792620991791b91510bf3585ecbd644c3bd5cad8abebfbda4f7"
        },
        {
            "kPublic": "0368cf0589d1cd438584eeb4e199be22badb53ee0ba2fc32847feab6925faa72d8",
            "kTwoPublic": "0284e5bfd42140ff67b049f2591cddda79cc4127d9169c36333955d6ce607ec261"
        },
        {
            "kPublic": "03e5e0a75e10bf73bf5ae2346cc877e64d768f1bddc37ff10ef9526976b8151754",
            "kTwoPublic": "02aa69c82633e6405a6b97c4a0f07a55b482e3b5f20981084173c618412c30b481"
        },
        {
            "kPublic": "039125bf3b2f669291a0501f1d5797deff6ce42a648d54f77e6254cadcbac733a1",
            "kTwoPublic": "031f88b05a910e76e11e26127e51ce6a5851fa96afd940c9692df1d142cdc84d36"
        },
        {
            "kPublic": "024c4a1cd29a98f3fca80547c6fa35dbaa890c01bc04133e52d6f4c9bffcaadbed",
            "kTwoPublic": "02c6a2d1ac562938540d635d42559da28d08f74364b9a45787131b1237137d94e0"
        },
        {
            "kPublic": "024f40609211ba54be8a3ef9a861f66596e05ed98eb7af274d1bdcbea93ea75990",
            "kTwoPublic": "02302b9ee73aca6272fce2266b7e82db12634b07bf98d296496a4b40e1ea20da4a"
        },
        {
            "kPublic": "039b459fb58a440c49eff2cda63f5e6b402ca227c5fbff47f9e7d51fa60db9cd49",
            "kTwoPublic": "020a4b4b7eeda8cadc1368c732c1e77c103bbfcab167baae9786c8f639ecadceaa"
        },
        {
            "kPublic": "026de0589b63bf76d2c839a18e1571ec6eac04fcb948ed6a183f595fc57e034341",
            "kTwoPublic": "034827d43d281de88f9b1e39b4ab0a62cd2efb04a7c70d132e5a40278d23397aea"
        },
        {
            "kPublic": "03d3a4b63963f50dd3811d4ae751b8e4c22a25c4f78088f43f04572439e15af15a",
            "kTwoPublic": "026f0ae313624e2d15fff828f9f1dd6ba2df0bf5920be2f23197eddfe3066bdf00"
        },
        {
            "kPublic": "0350a8a2b98550019b3d7cf7e100df0e33ed180219c4955ef2f22d16d6a4758d69",
            "kTwoPublic": "02c7f73398f4872d719be0871d3ce27169812eee95b473087924fd2c978747e444"
        },
        {
            "kPublic": "0322f6da4602fe7d7c91d5a5f3d8c3aaef86b5a639e3973757c44632ea6ff5f4fd",
            "kTwoPublic": "037d27c6a4b550f6ff782455cbff330eda7dabec884eaf7fd797f0c5cd6d4d0153"
        },
        {
            "kPublic": "02656322a5930ae49dbc3af9efa8388b5442cba79361ec0f06395897a1f237d7f8",
            "kTwoPublic": "03fa9406720f348421a1970919980ce4a23dca61db1d8fad8fe00f261ac4a60dba"
        }
    ],
    "generatedAddress": "bc1qa4w830n2c4v0j0usehg38xlr58xltxml3cyphrgz0kp4292yw7aqrsqhcr" 
}
```

**Request**

{% tabs %}
{% tab title="Curl" %}
```url
curl --location 'https://relay.sspwallet.io/v1/sync' \
--header 'Content-Type: application/json' \
--data '{
    "chain": "btc", 
    "walletIdentity": "1Kys4uBjfqcb3DrtkAs8Gc8YXEmDmeFESd", 
    "keyXpub": "Zpub75iXLooqV4JZNAC68gK6kAdNmAdY2wMMUeGxv9rZHHnWhU91EccH6r45CmYUnADR51p4H3aFkWLicnV5wj69GipUgA5eNM8RnYAtNpAxWb5", 
    "wkIdentity": "bc1q4q6nhujzlpll9e2mkjlqddmdfyvh4a89vqatsk0ju8jwt2vghf9sykaw0c", 
    "publicNonces": [
        {
            "kPublic": "027fa9f9c1e8cbd77799d39be7e06ea55dbe793fd1b74857e3a83e7de866e07027",
            "kTwoPublic": "037b87c588195c9883ca18b9e5c8acb156bf9ff4193dc6db381361e2ca2c386046"
        },
        {
            "kPublic": "03d1c7179c86752f6f0c24d726c43e42ef902461fb1ad805ffe06cae5a8ac05814",
            "kTwoPublic": "02ab3045c751591bb7ec2a236d9e42794f1692788f0f11f97164e3b02e75fd8d0d"
        },
        {
            "kPublic": "02c512ddf6fd8f5927adabb2c9ca38c22c6355bec6e27d5a3e01ca608eb4e40ce0",
            "kTwoPublic": "026b831445a028ac460fed5fb3579562c69ebdf9465414ec7ba7eb0a2bb8471840"
        },
        {
            "kPublic": "03c764f7c7f10e9935da59824954dcb1cd3870bfd7dd6b46b27e9b9b75fb51df90",
            "kTwoPublic": "03b5f5a9cd2cdfe7ea3621dc6b02fedb7a04093f9b32bf006577d6395360b583b9"
        },
        {
            "kPublic": "02ba82c7f0dec94fe54e47bd384554729a6a817d2ff875bfc11ff72e174453ca3b",
            "kTwoPublic": "034b84869ab98addafcb29bd0463741c03dcdf361d356bba3bf7bf8e666cbe6b54"
        },
        {
            "kPublic": "026fe4afabaae005ce1c9ba58b19b4a7e9f6934334bdecd5e10897a05b3015b569",
            "kTwoPublic": "020b6924a24966b27bdec49272dca1af5209387d9466f419c4ff57d63c1f6e23b2"
        },
        {
            "kPublic": "036ac536fdbfbdd3ab4b53b7fdbfb887e548f97fdeeeb68e8babe4fa44eef7a13f",
            "kTwoPublic": "02702b3d406b5eefc0183e7d2c734676595ff87f36c3abf41e23d30d9ce0c77f01"
        },
        {
            "kPublic": "0297c648c8aef9162bcb381e4e6fb3c80747d48965b974635923d36724fe52f77e",
            "kTwoPublic": "0227696fa75d105be1b799becdb55cdb94cd29f8b39a7f7fcd15a69a5fe6b28874"
        },
        {
            "kPublic": "022a4ab9d2c942784eb9fd0b57d367a55296b96ebb5c03d5da86aad5c18258e9f0",
            "kTwoPublic": "020f11d46c8ebb0a16e7af8b42b6f8ffa728e900f6f15e5d1f1ec5eb0ffb77ab7f"
        },
        {
            "kPublic": "034448b2a43053b493a6ede09adda28c5036e4c5b5cfd493dda1567fc8b2a0676f",
            "kTwoPublic": "02ff4f296aa4108ed317bd67c12c41a0a1cea8ca1477e8c93b0dfdbefb9c849668"
        },
        {
            "kPublic": "03eb6487f0984c629c4de0cec57b4cbfdfc7d411a068a47906984a9d71d2e3d0ca",
            "kTwoPublic": "03da21ac3cc833d7e5ad38b016f020455cf9ec09119f26d7be7fdcc122399ca841"
        },
        {
            "kPublic": "032ee70475b5a6e082fe0765eaed566e35f5f2169e7a9483bd6a82b0da7291a442",
            "kTwoPublic": "03133fa2a3731c2d1ffb3a51ce68405fa7059da0da8b9130a6c92ee7a443a20c99"
        },
        {
            "kPublic": "03145d07decc92e8119f2e6853e0ec09adb3f0b85e40aecc77e9e66d0a5ae92169",
            "kTwoPublic": "025791e4d08f500c3c468a4603e4409c45f4c4be5ffd84d859ec9798f38496d25c"
        },
        {
            "kPublic": "0331cc678b16484f6d4597e21b2b5a92501ae1691068abef9fb2e390c5b4aefa07",
            "kTwoPublic": "03c13c2eb0d7be04532913d758fb560348d5dc14e009d5b380d7e6a2194e288e2f"
        },
        {
            "kPublic": "02c35f88926be156aa7178061e6103925cd7b11796557d19a53e92bd9f667c195f",
            "kTwoPublic": "0353a213b38556ee95eb6fc72b137660c392b07e42f8cd802d626556d763d7313e"
        },
        {
            "kPublic": "03f38bd700f7c0a833d328d7adf33ea1c33752d95fc3794bbe3110f5ebb4cd7a14",
            "kTwoPublic": "03c7768aa2901b046bbee907691de2bf36ee9423d6410f84788027591e1d8f4edf"
        },
        {
            "kPublic": "02a9758e0d8741e1732e9c123e8c67634bfef7e52c9edd8befffd4e456707d1836",
            "kTwoPublic": "021d5022d12a5a20318ddad8827af1c6e2308c4710600fad63b85b2021224d35ea"
        },
        {
            "kPublic": "02bffebd506b888c62ade28245d9ebb3c8353d48baf749067f37973770a15888e3",
            "kTwoPublic": "02ca23f34e0443565c3c91627b30a0e4fdb3c99d3477efea354f47a22e6900ee75"
        },
        {
            "kPublic": "031d1963e434ce049e0c82199103b0ee5cf1f6f04437dac33956041d04e73446af",
            "kTwoPublic": "032fad849c38b555883c341ea1b0362f5135a75944bd0a15529825b0b8457988d6"
        },
        {
            "kPublic": "039193ee2267106322acb712950700db43850be7fc3c297ce5d51033d3d1da72f5",
            "kTwoPublic": "0317da113a69377ae1807050becb80325e71146c9133a32820bdf56cb7b64ba5ab"
        },
        {
            "kPublic": "032f085acb7109ef7a772d96367cf34ffda6e485aaced64cffc19cca57455472f8",
            "kTwoPublic": "02b2eb3c82361fee262f61a887d900daaa0fa03b369e14551a972c7db0efab5149"
        },
        {
            "kPublic": "03e08233a625f1086836d4274398a16b471e187cea3d3eb841b3f8e777e6f385b1",
            "kTwoPublic": "02aa9322e7b4f8bbb0aa6e7527b07761aff79b6c7e9b471ba64d49e1e2d0cfb1ad"
        },
        {
            "kPublic": "03a6801c45aa35be7dbf630fbb16d4150433320ada2af73016b821a22a3ffdf351",
            "kTwoPublic": "03331f7260f679b90f63211b46b7938322a37cc6897020a03802a9f4b976fb71bf"
        },
        {
            "kPublic": "032b01f558f808cc6b59f5048b1ab9fa91889e9e5d01c17cd24cfa679784e9bc18",
            "kTwoPublic": "029de37a609ba223ade79eae7422571f73305e739f6c3895dfefa5d1bb10addd38"
        },
        {
            "kPublic": "03ebf9f9cd36f591a3dc6875ae673ef9731610326d72f9b9286cc43c45ab24293e",
            "kTwoPublic": "0245400480b22a156194399db6a367068df8e56c7f0033dec53618b9fd2ca94796"
        },
        {
            "kPublic": "03eaa11310bb09f65abf897ed2fc73d3b4f66e3f7b807ef3d73cbe0f54ce686c7a",
            "kTwoPublic": "031c151da1e78ba5620020a73f5b1dd9311a00b95d9c8ef7950ffe4199008c26f3"
        },
        {
            "kPublic": "03165c9d432229b2dcfd580d618f210daf2e7ef44dda087788edcb2e378a7b89e3",
            "kTwoPublic": "03c8a5e37e3488b00282d2b13caff7f91e0915d076cce1e2f0d2d8a03b91ff6dde"
        },
        {
            "kPublic": "02f7a04407cad6860e8d1f804a38f0da380f84be806ff415fedb9c533656ca9207",
            "kTwoPublic": "02c80dde44469e9c794ec5708edb6d05262d62dec906be8a0d146adc3ba4e05448"
        },
        {
            "kPublic": "03f4d043c18c1f1842074f9859d82e0000d3f2d7bdc1f75a2a3e185950be945659",
            "kTwoPublic": "02185c61b039a55c8a56aca5e25bd2cc8e3f3761042da9ce245aeab5d4e45b1020"
        },
        {
            "kPublic": "02bf52bec05bc3b344905410e7ac230d19ceabd49ec9e793c18db6d8d51d8c922b",
            "kTwoPublic": "0331952f8b5feecae6543b514cb1929e55f477463dbaf3e90cac695e481dcbf3e0"
        },
        {
            "kPublic": "02f2d5bfdfd231a8c82f391e7cb975572de8cfdfcef8028affa7843225656ea6ac",
            "kTwoPublic": "0283e0356a437f6218f865a409632737364d1fa7eef1b391d83ba09713ad39748e"
        },
        {
            "kPublic": "0209b403e2f7e3565d09dd32aa7ba389456839b28bfcaa1d93aa3f6b52b509339e",
            "kTwoPublic": "02966078a7e3bf88af05cec889268dbb4510d63193bc4db8cfc516a3dd3e77577a"
        },
        {
            "kPublic": "025c4aa8a7b1e88a10a0983df1269cf9908b0bc15a3b1d32e4888eeb057c7e01a9",
            "kTwoPublic": "02d8e80845d57e340bd6ed1eb3b9192878cc220b6e4c60e2ee36ded22a36bdbbbc"
        },
        {
            "kPublic": "02022b9806d8f952225a48a2de4ceedbe4a1d2b4b57448153d9c564e1a30251f6a",
            "kTwoPublic": "02007a93caec5a514dce9b14b9f26774e2c790af398cd4561e0790a200c005db10"
        },
        {
            "kPublic": "02702968776d33e53adb722a41291b166984f3fd65c5e0335f75d8c98f13289ad3",
            "kTwoPublic": "02658ce5fa3c905a953490e8c2ef567343525446c8c60e4d115ac07ff867b248d6"
        },
        {
            "kPublic": "0364f0fb53a5dcd03bb4110a5d80bfbb5c7b853bcdb31a3fb49cc8b8dbc658795a",
            "kTwoPublic": "03bfbb44069fc3e55b304488869927a6b76d7db9fc58ac9094039f50e49b679962"
        },
        {
            "kPublic": "0343dc67ce850f932c5cbddb6dafa42899fc89a87c130f66397807cf7181a01b9c",
            "kTwoPublic": "02470145d4b50c477a06e5b06baa5f58f5b006dfd68c98923828db485261ef1132"
        },
        {
            "kPublic": "02ee9d8e52ad05461fad7045d58d93af849ab9fe7847bbc2bc22384e896697f871",
            "kTwoPublic": "02a11c7d1d624b18d2e24afdd6303f984a0f0208a294dc93d8bbda66f72c03b88c"
        },
        {
            "kPublic": "0266d99ff948390ce7bf789a4813848af0e96ce81a6342f7048bcf0bea1fbf6e07",
            "kTwoPublic": "02a960aa9509f41792620991791b91510bf3585ecbd644c3bd5cad8abebfbda4f7"
        },
        {
            "kPublic": "0368cf0589d1cd438584eeb4e199be22badb53ee0ba2fc32847feab6925faa72d8",
            "kTwoPublic": "0284e5bfd42140ff67b049f2591cddda79cc4127d9169c36333955d6ce607ec261"
        },
        {
            "kPublic": "03e5e0a75e10bf73bf5ae2346cc877e64d768f1bddc37ff10ef9526976b8151754",
            "kTwoPublic": "02aa69c82633e6405a6b97c4a0f07a55b482e3b5f20981084173c618412c30b481"
        },
        {
            "kPublic": "039125bf3b2f669291a0501f1d5797deff6ce42a648d54f77e6254cadcbac733a1",
            "kTwoPublic": "031f88b05a910e76e11e26127e51ce6a5851fa96afd940c9692df1d142cdc84d36"
        },
        {
            "kPublic": "024c4a1cd29a98f3fca80547c6fa35dbaa890c01bc04133e52d6f4c9bffcaadbed",
            "kTwoPublic": "02c6a2d1ac562938540d635d42559da28d08f74364b9a45787131b1237137d94e0"
        },
        {
            "kPublic": "024f40609211ba54be8a3ef9a861f66596e05ed98eb7af274d1bdcbea93ea75990",
            "kTwoPublic": "02302b9ee73aca6272fce2266b7e82db12634b07bf98d296496a4b40e1ea20da4a"
        },
        {
            "kPublic": "039b459fb58a440c49eff2cda63f5e6b402ca227c5fbff47f9e7d51fa60db9cd49",
            "kTwoPublic": "020a4b4b7eeda8cadc1368c732c1e77c103bbfcab167baae9786c8f639ecadceaa"
        },
        {
            "kPublic": "026de0589b63bf76d2c839a18e1571ec6eac04fcb948ed6a183f595fc57e034341",
            "kTwoPublic": "034827d43d281de88f9b1e39b4ab0a62cd2efb04a7c70d132e5a40278d23397aea"
        },
        {
            "kPublic": "03d3a4b63963f50dd3811d4ae751b8e4c22a25c4f78088f43f04572439e15af15a",
            "kTwoPublic": "026f0ae313624e2d15fff828f9f1dd6ba2df0bf5920be2f23197eddfe3066bdf00"
        },
        {
            "kPublic": "0350a8a2b98550019b3d7cf7e100df0e33ed180219c4955ef2f22d16d6a4758d69",
            "kTwoPublic": "02c7f73398f4872d719be0871d3ce27169812eee95b473087924fd2c978747e444"
        },
        {
            "kPublic": "0322f6da4602fe7d7c91d5a5f3d8c3aaef86b5a639e3973757c44632ea6ff5f4fd",
            "kTwoPublic": "037d27c6a4b550f6ff782455cbff330eda7dabec884eaf7fd797f0c5cd6d4d0153"
        },
        {
            "kPublic": "02656322a5930ae49dbc3af9efa8388b5442cba79361ec0f06395897a1f237d7f8",
            "kTwoPublic": "03fa9406720f348421a1970919980ce4a23dca61db1d8fad8fe00f261ac4a60dba"
        }
    ], 
    "generatedAddress": "bc1qa4w830n2c4v0j0usehg38xlr58xltxml3cyphrgz0kp4292yw7aqrsqhcr" 
}'
```
{% endtab %}
{% endtabs %}

**Response**

| Parameter        | Description                          |
| ---------------- | ------------------------------------ |
| chain            | chain where the id is associated     |
| walletIdentity   | id being used to identify the wallet |
| keyXpub          | xpub key of the wallet               |
| wkIdentity       | wallet key value                     |
| publicNonces     | public nonces value                  |
| generatedAddress | address associated with the wallet   |
| createdAt        | creation date timestamp              |
| expireAt         | expiration date timestamp            |

{% tabs %}
{% tab title="200 - Success" %}
```json
{
    "status": "success",
    "data": {
        "chain": "btc",
        "walletIdentity": "1Kys4uBjfqcb3DrtkAs8Gc8YXEmDmeFESd",
        "keyXpub": "Zpub75iXLooqV4JZNAC68gK6kAdNmAdY2wMMUeGxv9rZHHnWhU91EccH6r45CmYUnADR51p4H3aFkWLicnV5wj69GipUgA5eNM8RnYAtNpAxWb5",
        "wkIdentity": "bc1q4q6nhujzlpll9e2mkjlqddmdfyvh4a89vqatsk0ju8jwt2vghf9sykaw0c",
        "generatedAddress": "bc1qa4w830n2c4v0j0usehg38xlr58xltxml3cyphrgz0kp4292yw7aqrsqhcr",
        "publicNonces": [
            {
                "kPublic": "027fa9f9c1e8cbd77799d39be7e06ea55dbe793fd1b74857e3a83e7de866e07027",
                "kTwoPublic": "037b87c588195c9883ca18b9e5c8acb156bf9ff4193dc6db381361e2ca2c386046"
            },
            {
                "kPublic": "03d1c7179c86752f6f0c24d726c43e42ef902461fb1ad805ffe06cae5a8ac05814",
                "kTwoPublic": "02ab3045c751591bb7ec2a236d9e42794f1692788f0f11f97164e3b02e75fd8d0d"
            },
            {
                "kPublic": "02c512ddf6fd8f5927adabb2c9ca38c22c6355bec6e27d5a3e01ca608eb4e40ce0",
                "kTwoPublic": "026b831445a028ac460fed5fb3579562c69ebdf9465414ec7ba7eb0a2bb8471840"
            },
            {
                "kPublic": "03c764f7c7f10e9935da59824954dcb1cd3870bfd7dd6b46b27e9b9b75fb51df90",
                "kTwoPublic": "03b5f5a9cd2cdfe7ea3621dc6b02fedb7a04093f9b32bf006577d6395360b583b9"
            },
            {
                "kPublic": "02ba82c7f0dec94fe54e47bd384554729a6a817d2ff875bfc11ff72e174453ca3b",
                "kTwoPublic": "034b84869ab98addafcb29bd0463741c03dcdf361d356bba3bf7bf8e666cbe6b54"
            },
            {
                "kPublic": "026fe4afabaae005ce1c9ba58b19b4a7e9f6934334bdecd5e10897a05b3015b569",
                "kTwoPublic": "020b6924a24966b27bdec49272dca1af5209387d9466f419c4ff57d63c1f6e23b2"
            },
            {
                "kPublic": "036ac536fdbfbdd3ab4b53b7fdbfb887e548f97fdeeeb68e8babe4fa44eef7a13f",
                "kTwoPublic": "02702b3d406b5eefc0183e7d2c734676595ff87f36c3abf41e23d30d9ce0c77f01"
            },
            {
                "kPublic": "0297c648c8aef9162bcb381e4e6fb3c80747d48965b974635923d36724fe52f77e",
                "kTwoPublic": "0227696fa75d105be1b799becdb55cdb94cd29f8b39a7f7fcd15a69a5fe6b28874"
            },
            {
                "kPublic": "022a4ab9d2c942784eb9fd0b57d367a55296b96ebb5c03d5da86aad5c18258e9f0",
                "kTwoPublic": "020f11d46c8ebb0a16e7af8b42b6f8ffa728e900f6f15e5d1f1ec5eb0ffb77ab7f"
            },
            {
                "kPublic": "034448b2a43053b493a6ede09adda28c5036e4c5b5cfd493dda1567fc8b2a0676f",
                "kTwoPublic": "02ff4f296aa4108ed317bd67c12c41a0a1cea8ca1477e8c93b0dfdbefb9c849668"
            },
            {
                "kPublic": "03eb6487f0984c629c4de0cec57b4cbfdfc7d411a068a47906984a9d71d2e3d0ca",
                "kTwoPublic": "03da21ac3cc833d7e5ad38b016f020455cf9ec09119f26d7be7fdcc122399ca841"
            },
            {
                "kPublic": "032ee70475b5a6e082fe0765eaed566e35f5f2169e7a9483bd6a82b0da7291a442",
                "kTwoPublic": "03133fa2a3731c2d1ffb3a51ce68405fa7059da0da8b9130a6c92ee7a443a20c99"
            },
            {
                "kPublic": "03145d07decc92e8119f2e6853e0ec09adb3f0b85e40aecc77e9e66d0a5ae92169",
                "kTwoPublic": "025791e4d08f500c3c468a4603e4409c45f4c4be5ffd84d859ec9798f38496d25c"
            },
            {
                "kPublic": "0331cc678b16484f6d4597e21b2b5a92501ae1691068abef9fb2e390c5b4aefa07",
                "kTwoPublic": "03c13c2eb0d7be04532913d758fb560348d5dc14e009d5b380d7e6a2194e288e2f"
            },
            {
                "kPublic": "02c35f88926be156aa7178061e6103925cd7b11796557d19a53e92bd9f667c195f",
                "kTwoPublic": "0353a213b38556ee95eb6fc72b137660c392b07e42f8cd802d626556d763d7313e"
            },
            {
                "kPublic": "03f38bd700f7c0a833d328d7adf33ea1c33752d95fc3794bbe3110f5ebb4cd7a14",
                "kTwoPublic": "03c7768aa2901b046bbee907691de2bf36ee9423d6410f84788027591e1d8f4edf"
            },
            {
                "kPublic": "02a9758e0d8741e1732e9c123e8c67634bfef7e52c9edd8befffd4e456707d1836",
                "kTwoPublic": "021d5022d12a5a20318ddad8827af1c6e2308c4710600fad63b85b2021224d35ea"
            },
            {
                "kPublic": "02bffebd506b888c62ade28245d9ebb3c8353d48baf749067f37973770a15888e3",
                "kTwoPublic": "02ca23f34e0443565c3c91627b30a0e4fdb3c99d3477efea354f47a22e6900ee75"
            },
            {
                "kPublic": "031d1963e434ce049e0c82199103b0ee5cf1f6f04437dac33956041d04e73446af",
                "kTwoPublic": "032fad849c38b555883c341ea1b0362f5135a75944bd0a15529825b0b8457988d6"
            },
            {
                "kPublic": "039193ee2267106322acb712950700db43850be7fc3c297ce5d51033d3d1da72f5",
                "kTwoPublic": "0317da113a69377ae1807050becb80325e71146c9133a32820bdf56cb7b64ba5ab"
            },
            {
                "kPublic": "032f085acb7109ef7a772d96367cf34ffda6e485aaced64cffc19cca57455472f8",
                "kTwoPublic": "02b2eb3c82361fee262f61a887d900daaa0fa03b369e14551a972c7db0efab5149"
            },
            {
                "kPublic": "03e08233a625f1086836d4274398a16b471e187cea3d3eb841b3f8e777e6f385b1",
                "kTwoPublic": "02aa9322e7b4f8bbb0aa6e7527b07761aff79b6c7e9b471ba64d49e1e2d0cfb1ad"
            },
            {
                "kPublic": "03a6801c45aa35be7dbf630fbb16d4150433320ada2af73016b821a22a3ffdf351",
                "kTwoPublic": "03331f7260f679b90f63211b46b7938322a37cc6897020a03802a9f4b976fb71bf"
            },
            {
                "kPublic": "032b01f558f808cc6b59f5048b1ab9fa91889e9e5d01c17cd24cfa679784e9bc18",
                "kTwoPublic": "029de37a609ba223ade79eae7422571f73305e739f6c3895dfefa5d1bb10addd38"
            },
            {
                "kPublic": "03ebf9f9cd36f591a3dc6875ae673ef9731610326d72f9b9286cc43c45ab24293e",
                "kTwoPublic": "0245400480b22a156194399db6a367068df8e56c7f0033dec53618b9fd2ca94796"
            },
            {
                "kPublic": "03eaa11310bb09f65abf897ed2fc73d3b4f66e3f7b807ef3d73cbe0f54ce686c7a",
                "kTwoPublic": "031c151da1e78ba5620020a73f5b1dd9311a00b95d9c8ef7950ffe4199008c26f3"
            },
            {
                "kPublic": "03165c9d432229b2dcfd580d618f210daf2e7ef44dda087788edcb2e378a7b89e3",
                "kTwoPublic": "03c8a5e37e3488b00282d2b13caff7f91e0915d076cce1e2f0d2d8a03b91ff6dde"
            },
            {
                "kPublic": "02f7a04407cad6860e8d1f804a38f0da380f84be806ff415fedb9c533656ca9207",
                "kTwoPublic": "02c80dde44469e9c794ec5708edb6d05262d62dec906be8a0d146adc3ba4e05448"
            },
            {
                "kPublic": "03f4d043c18c1f1842074f9859d82e0000d3f2d7bdc1f75a2a3e185950be945659",
                "kTwoPublic": "02185c61b039a55c8a56aca5e25bd2cc8e3f3761042da9ce245aeab5d4e45b1020"
            },
            {
                "kPublic": "02bf52bec05bc3b344905410e7ac230d19ceabd49ec9e793c18db6d8d51d8c922b",
                "kTwoPublic": "0331952f8b5feecae6543b514cb1929e55f477463dbaf3e90cac695e481dcbf3e0"
            },
            {
                "kPublic": "02f2d5bfdfd231a8c82f391e7cb975572de8cfdfcef8028affa7843225656ea6ac",
                "kTwoPublic": "0283e0356a437f6218f865a409632737364d1fa7eef1b391d83ba09713ad39748e"
            },
            {
                "kPublic": "0209b403e2f7e3565d09dd32aa7ba389456839b28bfcaa1d93aa3f6b52b509339e",
                "kTwoPublic": "02966078a7e3bf88af05cec889268dbb4510d63193bc4db8cfc516a3dd3e77577a"
            },
            {
                "kPublic": "025c4aa8a7b1e88a10a0983df1269cf9908b0bc15a3b1d32e4888eeb057c7e01a9",
                "kTwoPublic": "02d8e80845d57e340bd6ed1eb3b9192878cc220b6e4c60e2ee36ded22a36bdbbbc"
            },
            {
                "kPublic": "02022b9806d8f952225a48a2de4ceedbe4a1d2b4b57448153d9c564e1a30251f6a",
                "kTwoPublic": "02007a93caec5a514dce9b14b9f26774e2c790af398cd4561e0790a200c005db10"
            },
            {
                "kPublic": "02702968776d33e53adb722a41291b166984f3fd65c5e0335f75d8c98f13289ad3",
                "kTwoPublic": "02658ce5fa3c905a953490e8c2ef567343525446c8c60e4d115ac07ff867b248d6"
            },
            {
                "kPublic": "0364f0fb53a5dcd03bb4110a5d80bfbb5c7b853bcdb31a3fb49cc8b8dbc658795a",
                "kTwoPublic": "03bfbb44069fc3e55b304488869927a6b76d7db9fc58ac9094039f50e49b679962"
            },
            {
                "kPublic": "0343dc67ce850f932c5cbddb6dafa42899fc89a87c130f66397807cf7181a01b9c",
                "kTwoPublic": "02470145d4b50c477a06e5b06baa5f58f5b006dfd68c98923828db485261ef1132"
            },
            {
                "kPublic": "02ee9d8e52ad05461fad7045d58d93af849ab9fe7847bbc2bc22384e896697f871",
                "kTwoPublic": "02a11c7d1d624b18d2e24afdd6303f984a0f0208a294dc93d8bbda66f72c03b88c"
            },
            {
                "kPublic": "0266d99ff948390ce7bf789a4813848af0e96ce81a6342f7048bcf0bea1fbf6e07",
                "kTwoPublic": "02a960aa9509f41792620991791b91510bf3585ecbd644c3bd5cad8abebfbda4f7"
            },
            {
                "kPublic": "0368cf0589d1cd438584eeb4e199be22badb53ee0ba2fc32847feab6925faa72d8",
                "kTwoPublic": "0284e5bfd42140ff67b049f2591cddda79cc4127d9169c36333955d6ce607ec261"
            },
            {
                "kPublic": "03e5e0a75e10bf73bf5ae2346cc877e64d768f1bddc37ff10ef9526976b8151754",
                "kTwoPublic": "02aa69c82633e6405a6b97c4a0f07a55b482e3b5f20981084173c618412c30b481"
            },
            {
                "kPublic": "039125bf3b2f669291a0501f1d5797deff6ce42a648d54f77e6254cadcbac733a1",
                "kTwoPublic": "031f88b05a910e76e11e26127e51ce6a5851fa96afd940c9692df1d142cdc84d36"
            },
            {
                "kPublic": "024c4a1cd29a98f3fca80547c6fa35dbaa890c01bc04133e52d6f4c9bffcaadbed",
                "kTwoPublic": "02c6a2d1ac562938540d635d42559da28d08f74364b9a45787131b1237137d94e0"
            },
            {
                "kPublic": "024f40609211ba54be8a3ef9a861f66596e05ed98eb7af274d1bdcbea93ea75990",
                "kTwoPublic": "02302b9ee73aca6272fce2266b7e82db12634b07bf98d296496a4b40e1ea20da4a"
            },
            {
                "kPublic": "039b459fb58a440c49eff2cda63f5e6b402ca227c5fbff47f9e7d51fa60db9cd49",
                "kTwoPublic": "020a4b4b7eeda8cadc1368c732c1e77c103bbfcab167baae9786c8f639ecadceaa"
            },
            {
                "kPublic": "026de0589b63bf76d2c839a18e1571ec6eac04fcb948ed6a183f595fc57e034341",
                "kTwoPublic": "034827d43d281de88f9b1e39b4ab0a62cd2efb04a7c70d132e5a40278d23397aea"
            },
            {
                "kPublic": "03d3a4b63963f50dd3811d4ae751b8e4c22a25c4f78088f43f04572439e15af15a",
                "kTwoPublic": "026f0ae313624e2d15fff828f9f1dd6ba2df0bf5920be2f23197eddfe3066bdf00"
            },
            {
                "kPublic": "0350a8a2b98550019b3d7cf7e100df0e33ed180219c4955ef2f22d16d6a4758d69",
                "kTwoPublic": "02c7f73398f4872d719be0871d3ce27169812eee95b473087924fd2c978747e444"
            },
            {
                "kPublic": "0322f6da4602fe7d7c91d5a5f3d8c3aaef86b5a639e3973757c44632ea6ff5f4fd",
                "kTwoPublic": "037d27c6a4b550f6ff782455cbff330eda7dabec884eaf7fd797f0c5cd6d4d0153"
            },
            {
                "kPublic": "02656322a5930ae49dbc3af9efa8388b5442cba79361ec0f06395897a1f237d7f8",
                "kTwoPublic": "03fa9406720f348421a1970919980ce4a23dca61db1d8fad8fe00f261ac4a60dba"
            }
        ],
        "createdAt": "2025-02-19T11:33:16.036Z",
        "expireAt": "2025-02-19T11:48:16.036Z"
    }
}
```
{% endtab %}

{% tab title="400  - Failed Response" %}
```json
// List of messages:
// Invalid Chain specified
// Invalid Wallet identity specified
// Invalid XPUB of Key specified
// Invalid SSP Identity specified
// Invalid generated address
// Generated address is invalid
// Invalid public nonces
// Invalid public nonce detected
// Public nonce is too long
// Too many public nonces submitted
// Failed to update synchronisation data

{
    "status": "error",
    "data": {
        "name": "Error",
        "message": "<message>"
    }
}
```
{% endtab %}
{% endtabs %}
