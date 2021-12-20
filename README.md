# Kommander apps

The purpose of this workspace is to demonstrate creation of custom apps while installing kommander

Steps:

1. Clone kommander application default repository

```
export KOMMANDER_VERSION=2.1.0
wget https://github.com/mesosphere/kommander-applications/archive/refs/tags/v${KOMMANDER_VERSION}.tar.gz
tar -xzvf v${KOMMANDER_VERSION}.tar.gz  

```

2. Clone this repository

```
git clone https://github.com/arbhoj/kommander-apps.git
```

3. Copy over contents of this repository into kommander-applications repository directory

```
cp -r kommander-apps/services kommander-applications-${KOMMANDER_VERSION}/
cp -r kommander-apps/common kommander-applications-${KOMMANDER_VERSION}/
```

4. Update kustomization.yaml in kommander-applications/common/helm-repositories to include the custom helmrepository used in this example

```
echo "  - keycloak-codecentric-helmrepo.yaml" >> kommander-applications-${KOMMANDER_VERSION}/common/helm-repositories/kustomization.yaml
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
./kommander install --kommander-applications-repository kommander-applications-${KOMMANDER_VERSION}/ --installer-config install.yaml 
```

8. Verify the custom apps got deployed once kommander install process completes

```
kubectl get hr -A | grep keycloak
```
