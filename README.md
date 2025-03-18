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

## Diagrama de Arquitetura (Mermaid)
```bash
flowchart TD
    A[Repositório GitHub/Azure Repos] B[CI Pipeline (Build & Test)]
    B --> C[Docker Build e Testes Automatizados]
    C --> D[Push para ACR]
    D --> E[CD Pipeline (Deploy Automático)]
    E --> F[ACI]
    F --> G[Azure Monitor & Log Analytics]
```

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
   docker push sprint3.azurecr.io/odontoprev:latest

10. az acr update -n sprint3 --admin-enabled true

11. az acr credential show --name sprint3

12. docker login sprint3.azurecr.io --username sprint3 --password 0VQHpF7CLDrCxKn4OBsA/uW8TaJfZxJbTHe7F+vXSj+ACRBXEmvp


13. az container create --resource-group sprint3 --name sprint3 --image sprint3.azurecr.io/odontoprev:latest --registry-login-server sprint3.azurecr.io --registry-username sprint3 --registry-password 0VQHpF7CLDrCxKn4OBsA/uW8TaJfZxJbTHe7F+vXSj+ACRBXEmvp --dns-name-label odontoprev-aci-unique --ports 80 --os-type Linux --cpu 1 --memory 1.5




