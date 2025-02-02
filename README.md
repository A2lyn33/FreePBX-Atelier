# FreePBX - Atelier d'installation et de configuration  :telephone_receiver:

![CjEbGDI2lnJZC1XB1Hc2vavKRmbQ7LMN](https://github.com/user-attachments/assets/b7675eb0-1dfb-4774-b71f-d26a2fbc0e5e)


---

## :information_source: Introduction 
FreePBX est une plateforme de communication **open-source** basée sur Asterisk. Elle offre une interface web intuitive pour gérer la VoIP.  
Dans cet atelier, tu exploreras l’installation, la configuration et la maintenance de FreePBX. Tu vas aussi apprendre à créer des **pools d’appel** pour gérer les flux d’appels et configurer les **renvois d’appel** pour assurer la continuité des communications.

L’objectif est de te fournir les compétences nécessaires pour **optimiser un système de téléphonie d’entreprise**.

---

## :dart: Objectifs

- :white_check_mark: Naviguer dans l’administration de FreePBX  
- :white_check_mark: Savoir créer des lignes associées à des utilisateurs  
- :white_check_mark: Savoir faire des renvois d’appels  
- :white_check_mark: Créer un pool d’appels  

---

## :clipboard: Sommaire

1. [Étape 1 - Prérequis](#étape-1---prérequis)  
2. [Étape 2 - Création de lignes](#étape-2---création-de-lignes)  
3. [Étape-3 - Renvois dappel](#étape-3---renvois-dappel)  
4. [Étape-4 - Pool-dappel](#étape-4---pool-dappel)  

---

## :heavy_check_mark: Étape 1 - Prérequis

Pour cet atelier, tu as besoin :

1. **Un hyperviseur** comme Virtualbox pour pouvoir créer des VM  
2. **1 VM IPBX avec FreePBX** installé en anglais et mis à jour, avec :  
   - 2 cartes réseaux :  
     - Une carte réseau en **Réseau interne** avec l’adresse IP `172.16.10.35/24`  
     - Une carte en **NAT en DHCP**  
   - Les différents modules installés sont à jour  
3. **3 VM** : `Client1`, `Client2`, et `Client3` avec :  
   - Une carte réseau en **Réseau interne** avec respectivement l’adresse IP  
     - `172.16.10.101/24`  
     - `172.16.10.102/24`  
     - `172.16.10.103/24`  
   - Un softphone **3CX** installé sur chaque VM  

> **Note :**  
> Les expérimentations pratiques ont été testées avec :  
> - OS Windows Server 2022  
> - Windows 10  
> - FreePBX sur une distribution Red Hat  
> - VirtualBox 7 (sur un hôte Ubuntu 22.04 LTS)  
>
> Elles peuvent être reproduites sur d’autres versions ou d’autres environnements, mais **des différences peuvent apparaître**.

---

## :microscope: Étape 2 - Création de lignes

Sur l’IPBX, **crée les lignes suivantes** et configure-les sur les machines correspondantes :

| Poste client | Numéro de ligne | Nom               | Mot de passe |
|--------------|-----------------|-------------------|--------------|
| Client 1     | 33101           | Stéphanie Morin   | 1234         |
| Client 2     | 33102           | Joël Lerobillard  | 1234         |
| Client 3     | 33103           | Jean Kyrin        | 1234         |

**Rappel :**  
- Connecte-toi sur **FreePBX en web**  
- Va dans **Applications -> Extensions** pour créer les comptes SIP  

---

## :microscope: Étape 3 - Renvois d’appel

Tu vas faire un renvoi d’appel du poste `33102` vers le poste `33103`.

1. Sur FreePBX, va dans **Applications -> FollowMe** et clique sur le crayon à côté du numéro `33102`.  
2. Dans le menu d’édition :  
   - **Follow-Me List** : mets le numéro vers lequel renvoyer, ici `33103`  
   - **Initial Ring Time** : temps avant le transfert d’appel, mets `5` secondes  
   - **Follow-Me Ring Time** : temps (ajouté à l’Initial Ring Time) avant que l’appel s’arrête, mets `10`  
   - **Enable Followme** : mets **Yes** pour activer le transfert d’appel  
3. Clique sur **Submit** puis **Apply Config**  

Pour valider le transfert d’appel :  
- Appelle le poste `33102` à partir du `33101`.  
- Au bout de 5 secondes, l’appel doit être transféré sur le poste `33103`.  

---

## :microscope: Étape 4 - Pool d’appel

1. **Désactive** le transfert d’appel fait précédemment.  
2. Maintenant, tu vas créer un numéro d’appel **Help-Desk** sur le numéro `33003`.  
   - Les 2 postes `33102` et `33103`, inclus dans ce pool d’appel, doivent sonner en même temps.  

   1. Va dans **Applications -> Ring Groups** et clique sur **Add Ring Group**.  
   2. Dans le menu :  
      - **Ring Group Number** : `33003`  
      - **Group Description** : `Help-Desk`  
      - **Extension List** : `33102` et `33103`  
      - **Ring Strategy** : `ringall`  
      - **Ring Time** : `15`  
      - **Destination if no answer** : `Terminate Call - Hangup`  
   3. Clique sur **Submit** puis **Apply Config**  

3. Pour vérifier la configuration, appelle le `33003` à partir du `33101`.  
   - Les 2 autres postes doivent **sonner en même temps**.

---

## :tada: Conclusion

Félicitations, tu es allé(e) plus loin dans la configuration de FreePBX !  
Continue à explorer les autres possibilités de configuration à partir de ces deux exemples.  

---

> **Pour aller plus loin :**  
> - Explore la configuration des **trunks** pour connecter FreePBX à un fournisseur SIP externe.  
> - Découvre les fonctionnalités avancées comme les **IVR** (Interactive Voice Response), les **queues** et les **annonces**.  
> - Expérimente la **sécurisation** de FreePBX (pare-feu, fail2ban, etc.).  

**Fin de l’atelier** :sparkles:

