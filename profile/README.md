# BBDW · Do incidente ao pull request

Uma loja de demonstração para acompanhar como Sentry, GitHub Actions e Copilot CLI ajudam a investigar incidentes e propor correções. Produtos e pedidos são fictícios; não há cobrança real.

## O fluxo do workshop

**Incidente no Sentry → issue no GitHub → investigação com Copilot → verificação da aplicação → PR em rascunho.**

O agente consulta o incidente, lê os contratos do serviço e propõe uma correção com um teste de regressão. Se a investigação indicar responsabilidade de outro serviço, o Patchwork cria uma issue vinculada nesse repositório, onde a hipótese será confirmada. A issue de origem permanece aberta.

Cada execução usa um único job. A verificação completa acontece uma vez após a correção; a publicação depende de sua aprovação. O PR aguarda revisão humana, sem merge ou deploy automáticos.

## Repositórios

| Repositório | Responsabilidade |
| --- | --- |
| [patchwork](https://github.com/bb-dw-26/patchwork) | Workflow reutilizável, skills, integração Sentry e publicação |
| [bbdw-storefront](https://github.com/bb-dw-26/bbdw-storefront) | Interface da loja em React e TypeScript |
| [bbdw-checkout](https://github.com/bb-dw-26/bbdw-checkout) | Serviço de pedidos e endereços em NestJS |
| [bbdw-catalog](https://github.com/bb-dw-26/bbdw-catalog) | Catálogo e preços em Go |

## Executar a loja

Com Git, Docker e Docker Compose disponíveis, clone os serviços como pastas irmãs:

```sh
git clone https://github.com/bb-dw-26/bbdw-catalog.git
git clone https://github.com/bb-dw-26/bbdw-checkout.git
git clone https://github.com/bb-dw-26/bbdw-storefront.git
cd bbdw-storefront
docker compose up --build -d
```

Abra [localhost:3000](http://localhost:3000). Ao terminar, execute `docker compose down` na mesma pasta. A [execução integrada](https://github.com/bb-dw-26/bbdw-storefront/blob/main/docs/EXECUCAO.md) também explica como iniciar os serviços sem Docker.

Para preparar a automação remota, siga a [configuração do Patchwork](https://github.com/bb-dw-26/patchwork#configuração), incluindo a integração Sentry, o App publicador e as permissões do Copilot. Iniciar a loja localmente não configura essas integrações.

## Acompanhar a investigação

Abra a issue do incidente e sua execução na aba **Actions**. Os logs mostram o prompt e a saída normal do Copilot, as consultas e a verificação da aplicação. Depois, leia o diagnóstico, o diff e o teste no PR proposto.

O [roteiro da apresentação](https://github.com/bb-dw-26/patchwork/blob/main/docs/ROTEIRO.md) orienta quem conduz a sessão. Os contratos de cada serviço estão em `docs/SPEC.md` nos respectivos repositórios.
