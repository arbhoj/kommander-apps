# Kommander apps

The purpose of this workspace is to demonstrate creation of custom apps while installing kommander

Steps:

1. Clone kommander application default repository

```
git clone https://github.com/mesosphere/kommander-applications.git 
```

2. Clone this repository

```
git clone https://github.com/arbhoj/kommander-apps.git
```

3. Copy over contents of this repository into kommander-applications repository directory

```
cp -r kommander-apps/services kommander-applications/
cp -r kommander-apps/common kommander-applications/
```

4. Update kustomization.yaml in kommander-applications/common/helm-repositories to include the custom helmrepository used in this example

```
echo "  - keycloak-codecentric-helmrepo.yaml" >> kommander-applications/common/helm-repositories/kustomization.yaml
```

5. Generate a kommander install config yaml if it does not exist already

```
./kommander install --init > install.yaml
```

6. Update install.yaml to instruct kommander installer to include the elastic and keycloak apps from the example

> Note: The -e flag is for Mac OS.

```
sed -i -e '/apps\:/a\
\ \ elasticsearch: null 
' install.yaml


sed -i -e '/apps\:/a\
\ \ keycloak: null 
' install.yaml

```

7. Install kommander with the custom settings
 
```
./kommander install --kommander-applications-repository kommander-applications/ --installer-config install.yaml 
```

8. Verify the custom apps got deployed once kommander install process completes

```
kubectl get hr -A | grep keycloak
```
