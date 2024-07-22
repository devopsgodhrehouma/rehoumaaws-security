## Tutoriel : Correction des URL AWS S3 pour l'accès avec la signature AWS Version 4

### Introduction

Lorsque vous essayez d'accéder à un fichier stocké sur AWS S3 en utilisant une URL, il peut arriver que vous rencontriez des erreurs de signature, comme indiqué dans le message d'erreur ci-dessous :

```
<Error>
   <Code>InvalidArgument</Code>
   <Message>Requests specifying Server Side Encryption with AWS KMS managed keys require AWS Signature Version 4.</Message>
   <ArgumentName>Authorization</ArgumentName>
   <ArgumentValue>null</ArgumentValue>
   <RequestId>Z7752W9JQ15Z36KW</RequestId>
   <HostId>...</HostId>
</Error>
```

Ce tutoriel vous guidera à travers la résolution de ce problème en générant une URL pré-signée avec AWS Signature Version 4.

### Étapes pour Reproduire le Problème

1. **Accès Direct à un Fichier S3 sans URL Pré-signée** :

   Essayer d'accéder directement à un fichier S3 avec une URL non signée peut générer une erreur de signature. Par exemple, accéder à l'URL suivante peut échouer :

   ```
   https://haythem1234564384441456.s3.amazonaws.com/clock.png
   ```

   Cette tentative d'accès direct renvoie une erreur de type `InvalidArgument`.

### Solution : Génération d'une URL Pré-signée

Pour résoudre ce problème, nous allons générer une URL pré-signée qui inclut une signature AWS valide. Cela peut être fait en utilisant le SDK boto3 pour Python ou l'AWS CLI.

#### 1. Utilisation de boto3 (Python)

Voici un script Python utilisant boto3 pour générer une URL pré-signée :

```python
import boto3
from botocore.exceptions import NoCredentialsError

def generate_presigned_url(bucket_name, object_name, expiration=3600):
    # Générer une URL pré-signée pour l'objet S3
    s3_client = boto3.client('s3')
    try:
        response = s3_client.generate_presigned_url('get_object',
                                                    Params={'Bucket': bucket_name,
                                                            'Key': object_name},
                                                    ExpiresIn=expiration)
    except NoCredentialsError:
        print("Identifiants non disponibles")
        return None

    # La réponse contient l'URL pré-signée
    return response

bucket_name = 'haythem1234564384441456'
object_name = 'clock.png'
url = generate_presigned_url(bucket_name, object_name)
print("URL pré-signée :", url)
```

Exécutez ce script pour obtenir une URL pré-signée valide. 

#### 2. Utilisation de l'AWS CLI

Vous pouvez également générer une URL pré-signée en utilisant l'AWS CLI avec la commande suivante :

```sh
aws s3 presign s3://haythem1234564384441456/clock.png --expires-in 3600
```

Cette commande générera une URL pré-signée valide pendant une heure.

### Conclusion

En suivant ces étapes, vous pouvez générer une URL pré-signée pour accéder aux fichiers stockés sur AWS S3, en évitant les erreurs de signature. Ce processus est essentiel pour sécuriser l'accès à vos objets S3 tout en permettant un accès contrôlé.

### Note pour les Étudiants

Lors de l'utilisation des services AWS, il est crucial de comprendre et de gérer les signatures et les permissions. Assurez-vous que vos identifiants AWS sont correctement configurés et que vous avez les autorisations nécessaires pour accéder aux ressources que vous souhaitez utiliser.

---

Ce tutoriel vous a guidé à travers la correction des erreurs de signature AWS et la génération d'URL pré-signées. Si vous avez des questions ou des difficultés, n'hésitez pas à les poser en classe.

# Annexe : 

## Tutoriel : Correction des URL AWS S3 avec Token

### Introduction

Dans ce tutoriel, nous allons voir comment corriger les URL AWS S3 en incluant un token de sécurité pour accéder aux objets stockés dans un bucket S3. Nous allons également expliquer comment générer une URL pré-signée pour éviter les erreurs de signature.

### Problème

Voici un exemple d'URL générée avec AWS S3 et comprenant un token de sécurité :

```
https://haythem1234564384441456.s3.us-east-1.amazonaws.com/clock.png?response-content-disposition=inline&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLWVhc3QtMSJHMEUCIB7DgS50%2FfonxhBFXIrMaF%2BpsREKWchqiLTk%2BdD11YVeAiEA3QFFxbAiewGIJdT8ZFt%2FHo8EryhspLU8oOV7%2F6Ks1Qsq%2BAIIgP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw3NDM0NjI1MTA4NTkiDD4oWFV2xwWAZtoN7irMAq%2FA8gMaRj5PWRLi5LBAqKoa81Zl4Iupb5DmabKECmHHoYBT0ndT6kjClIu3E5B66AmcnXZTTWw8HP8kUDwvWXw2QcyOs7yBvW4WwGVmIEsK2J3%2F2Ux6g7elVB0uPsyYKSO5EaKu7lh9%2BWQSbcSlsNL8VGWtvEo6Z29xCOgkXdyceQksbW%2FLIK4fVyPolhUxbj%2FoI5OeE7PYTIf1Qs3UPyHzoV%2FyGSTjpBjmyVv6qbenYaP0YstGvtwBmo7nVp2n1TQwvdGfHGFFkqIKDNp847Hf4wRP1nWRjfEvnYNu9DxwcMnqOLQLZhjpL9qBbAoK43wkhlNWnjI3wDGCCrCl5Ml4ayFCW9gv%2FVO9iG%2BoujP%2F8JBl5D6VE7cmSujy6U9c3kFq%2BYY6kX4Ea4QNhhRmpUHB92gxtikp23%2FmU0oiK4%2Ft4ayk%2Flp38vOFnCiJMInC%2B7QGOocCoaaZefbEbYM7kjnSKLr7j3srIzl9jtKVymbiLbxInHII2IYhaaKmQN1gSjUqposyP5wbYGSD4ZfEQFA2LRvxp4QfYl%2B1ntR1qcQxLiNO99zwDgxrtjKUnKOVgnFckjGfod1k0hx2LQAs4Sa%2F9FUieOQ1wuQZztZlUJYtrdrPMNE6djRJSvrWYnULKBfbC9sVUThMFGnD0qFMJIMq1JQdCDY3nYPDwvnPpIA6YOO%2FjkIohX9B33a3h3R0O0mdBoJQjx%2F1pWxIljBTtte4mB3cJqqqgGZzDtiGWvRtBpt3HBfdnM6itGT1Mbo7pE4%2FaaUTI3mNqfJMo17y%2FapQ1EVddMMWSaSEOLc%3D&X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Date=20240722T232200Z&X-Amz-SignedHeaders=host&X-Amz-Expires=299&X-Amz-Credential=ASIA22GORUEFVIZUJK6Z%2F20240722%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Signature=79f1619b80a5842e238af3e0bd25a00f9c41bc9b5a60adb33b76bf94bf96c7da
```

![image](https://github.com/user-attachments/assets/f62f0183-9aa5-4e47-b148-ab87f440cf64)

### Partie du Token

La partie du token de sécurité dans l'URL est la suivante :

```
&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLWVhc3QtMSJHMEUCIB7DgS50%2FfonxhBFXIrMaF%2BpsREKWchqiLTk%2BdD11YVeAiEA3QFFxbAiewGIJdT8ZFt%2FHo8EryhspLU8oOV7%2F6Ks1Qsq%2BAIIgP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw3NDM0NjI1MTA4NTkiDD4oWFV2xwWAZtoN7irMAq%2FA8gMaRj5PWRLi5LBAqKoa81Zl4Iupb5DmabKECmHHoYBT0ndT6kjClIu3E5B66AmcnXZTTWw8HP8kUDwvWXw2QcyOs7yBvW4WwGVmIEsK2J3%2F2Ux6g7elVB0uPsyYKSO5EaKu7lh9%2BWQSbcSlsNL8VGWtvEo6Z29xCOgkXdyceQksbW%2FLIK4fVyPolhUxbj%2FoI5OeE7PYTIf1Qs3UPyHzoV%2FyGSTjpBjmyVv6qbenYaP0YstGvtwBmo7nVp2n1TQwvdGfHGFFkqIKDNp847Hf4wRP1nWRjfEvnYNu9DxwcMnqOLQLZhjpL9qBbAoK43wkhlNWnjI3wDGCCrCl5Ml4ayFCW9gv%2FVO9iG%2BoujP%2F8JBl5D6VE7cmSujy6U9c3kFq%2BYY6kX4Ea4QNhhRmpUHB92gxtikp23%2FmU0oiK4%2Ft4ayk%2Flp38vOFnCiJMInC%2B7QGOocCoaaZefbEbYM7kjnSKLr7j3srIzl9jtKVymbiLbxInHII2IYhaaKmQN1gSjUqposyP5wbYGSD4ZfEQFA2LRvxp4QfYl%2B1ntR1qcQxLiNO99zwDgxrtjKUnKOVgnFckjGfod1k0hx2LQAs4Sa%2F9FUieOQ1wuQZztZlUJYtrdrPMNE6djRJSvrWYnULKBfbC9sVUThMFGnD0qFMJIMq1JQdCDY3nYPDwvnPpIA6YOO%2FjkIohX9B33a3h3R0O0mdBoJQjx%2F1pWxIljBTtte4mB3cJqqqgGZzDtiGWvRtBpt3HBfdnM6itGT1Mbo7pE4%2FaaUTI3mNqfJMo17y%2FapQ1EVddMMWSaSEOLc%3D
```

Malgré l'inclusion de ce token, vous pourriez ne pas être en mesure d'accéder à l'objet si la signature n'est pas correcte ou si d'autres paramètres de l'URL sont incorrects.

### Générer une URL Pré-signée

Pour résoudre ce problème, nous allons générer une URL pré-signée valide qui vous permettra d'accéder à l'objet sans erreurs. Voici comment procéder :

#### 1. Utilisation de boto3 (Python)

```python
import boto3
from botocore.exceptions import NoCredentialsError

def generate_presigned_url(bucket_name, object_name, expiration=3600):
    # Générer une URL pré-signée pour l'objet S3
    s

3_client = boto3.client('s3')
    try:
        response = s3_client.generate_presigned_url('get_object',
                                                    Params={'Bucket': bucket_name,
                                                            'Key': object_name},
                                                    ExpiresIn=expiration)
    except NoCredentialsError:
        print("Identifiants non disponibles")
        return None

    # La réponse contient l'URL pré-signée
    return response

bucket_name = 'haythem1234564384441456'
object_name = 'clock.png'
url = generate_presigned_url(bucket_name, object_name)
print("URL pré-signée :", url)
```

#### 2. Utilisation de l'AWS CLI

```sh
aws s3 presign s3://haythem1234564384441456/clock.png --expires-in 3600
```

### Conclusion

En générant une URL pré-signée, vous vous assurez que l'accès à votre objet S3 est sécurisé et que la signature est valide, évitant ainsi les erreurs d'accès.

### Note pour les Étudiants

Lorsque vous travaillez avec des ressources AWS, il est essentiel de bien comprendre la gestion des signatures et des permissions pour sécuriser vos accès et éviter les erreurs. Assurez-vous que vos configurations et permissions IAM sont correctement définies.

---

Ce tutoriel vous a guidé à travers la résolution des erreurs de signature AWS et la génération d'URL pré-signées. Si vous avez des questions ou des difficultés, n'hésitez pas à les poser en classe.
