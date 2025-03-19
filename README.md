## DEVOPS-ODONTOPREV

# Arquitetura DevOps para OdontoPrev

## Visão Geral
Esta solução integra práticas de DevOps para automação de integração, entrega e monitoramento da aplicação. Os principais componentes são:
- **Repositório (GitHub/Azure Repos):** Onde o código é versionado.
- **CI/CD Pipeline:** Automatiza build, testes, containerização e deploy.
- **Docker/ACR/ACI:** A aplicação é containerizada, armazenada no Azure Container Registry (ACR) e implantada via Azure Container Instances (ACI).
- **Monitoramento:** Logs e métricas são coletados via Azure Monitor e Log Analytics.

## Fluxo da Solução
1. **Commit & Build:** Desenvolvedores fazem commit no repositório; o pipeline de CI executa testes e constrói a imagem Docker.
2. **Publicação:** A imagem é enviada para o ACR.
3. **Deploy Automatizado:** O pipeline de CD retira a imagem do ACR e implanta no ACI.
4. **Monitoramento:** Logs e métricas são coletados para feedback e ajustes contínuos.


-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

## Deploy e Testes
### Instruções

1. git clone https://github.com/gabrieltf1901/.NET-ODONTOPREV

   cd Odontoprev

2. docker login 

3. docker build -t odontoprev:latest .

4. docker run -d -p 8080:80 odontoprev:latest

5. az login

6. az group create --name sprint3 --location eastus2

7. az acr create --resource-group sprint3 --name sprint3 --sku Basic

8. az acr login --name sprint3

9. docker tag odontoprev:latest sprint3.azurecr.io/odontoprev:latest
   
10. docker push sprint3.azurecr.io/odontoprev:latest

11. az acr update -n sprint3 --admin-enabled true

12. az acr credential show --name sprint3

13. docker login sprint3.azurecr.io --username sprint3 --password <senha>

14. az container create --resource-group sprint3 --name sprint3 --image sprint3.azurecr.io/odontoprev:latest --registry-login-server sprint3.azurecr.io --registry-username sprint3 --registry-password <senha> --dns-name-label odontoprev-aci-unique --ports 80 --os-type Linux --cpu 1 --memory 1.5

15. az container logs --resource-group sprint3 --name sprint3


### Remoção

1. az acr repository delete --name sprint3 --repository odontoprev --yes

2. az group delete --name sprint3 --yes --no-wait

3. az container delete --resource-group sprint3 --name sprint3 --yes




