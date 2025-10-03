# infr040 P6

## Connection au cluster Openshift
Pour vous connecter au cluster Openshift mis à disposition : 

### Pour Windows

Téléchargez sur votre machine le paquet OKD suivant : 
https://github.com/okd-project/okd/releases/download/4.19.0-okd-scos.19/openshift-client-windows-4.19.0-okd-scos.19.zip

Copiez le fichier oc.exe dans C:\Windows\System32

### Pour Linux 
Téléchargez sur votre machine le paquet OKD suivant : 
https://github.com/okd-project/okd/releases/download/4.19.0-okd-scos.19/openshift-client-linux-4.19.0-okd-scos.19.tar.gz

Dézippez et copiez le fichier oc dans /urs/bin
```
tar -xvf openshift-client-linux-4.19.0-okd-scos.19.tar.gz
cp oc /usr/bin/
```

### Pour tous les OS

Allez sur l'url https://console-openshift-console.apps.openshift.kakor.ovh authentifiez-vous avec l'utilisateur ipi-gp-x (le x étant le numéro de groupe) en choisisant "KeystoneIDP".

Une fois authentifié (attention à ne pas recharger la page même si l'affichage prends du temps), allez en haut à droite de la page et sélectionnez l'option "Copy login Command" et réauthentifiez vous. 

Cliquez sur le lien Display Token et copiez dans votre terminal sur VScode la ligne de commande qui à été donné avec oc. 

Pour vérifier que vous êtes authentifiés, lancez la commande ```oc get pod```

Afin de vous mettre dans le contexte du namespace qui sera provisionné sous le nom ipi-gp-x, vous pouvez lancer la commande :

```oc project project-gp-x```


# EXERCICE DE REMISE EN FORME 

Créez un deployment d'un seul conteneur dans le namespace de votre groupe (ex dans la doc https://kubernetes.io/docs/concepts/workloads/controllers/deployment/).

Ce deployment utilisera l'image publique : harbor.kakor.ovh/public/nginx:latest

Créez une configmap qui contiendra un fiochier index.html, monté sur le deployment pour afficher le texte de votre choix.

Créez un service lié à ce pod sur le port 80.

Créez la route openshift via le template suivant :

```
apiVersion: route.openshift.io/v1
kind: Route
metadata:
  name: <nomdevotreapp>
spec:
  host: app-gp-x.apps.openshift.kakor.ovh
  port:
    targetPort: <port de votre service>
  tls:
    insecureEdgeTerminationPolicy: Redirect
    termination: reencrypt
  to:
    kind: Service
    name: <nomduservice>
    weight: 100
  wildcardPolicy: None
```
