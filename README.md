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
```mermaid
flowchart TD
    A[Repositório GitHub/Azure Repos] -> B[CI Pipeline (Build & Test)]
    B --> C[Docker Build e Testes Automatizados]
    C --> D[Push para ACR]
    D --> E[CD Pipeline (Deploy Automático)]
    E --> F[ACI]
    F --> G[Azure Monitor & Log Analytics]
```
