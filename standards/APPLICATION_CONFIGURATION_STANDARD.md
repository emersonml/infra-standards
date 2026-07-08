# Application Configuration Standard

## Objetivo

Padronizar configuracoes basicas de aplicacoes implantadas na plataforma.

Este padrao cobre configuracoes funcionais que devem existir no bootstrap de
novas aplicacoes, especialmente idioma, localidade, regiao e timezone.

## Regionalizacao padrao

Classificacao:

- Governanca
- Padrao
- Operacao

Valores padrao para aplicacoes operadas no Brasil:

```text
language: pt_BR
locale: pt_BR
phone region: BR
timezone: America/Maceio
```

Regras:

- novas aplicacoes devem nascer com idioma e localidade padrao em `pt_BR`
  quando suportado;
- novas aplicacoes devem usar regiao telefonica `BR` quando suportado;
- novas aplicacoes devem usar timezone `America/Maceio` quando suportado;
- excecoes por cliente, contrato ou requisito tecnico devem ser documentadas;
- usuarios existentes nao devem ter preferencias pessoais alteradas sem
  aprovacao.

## Nextcloud

Para novas stacks Nextcloud, aplicar:

```text
default_language = pt_BR
default_locale = pt_BR
default_phone_region = BR
default_timezone = America/Maceio
```

Procedimento de referencia:

```bash
docker compose exec -T -u www-data app php occ config:system:set default_language --value=pt_BR
docker compose exec -T -u www-data app php occ config:system:set default_locale --value=pt_BR
docker compose exec -T -u www-data app php occ config:system:set default_phone_region --value=BR
docker compose exec -T -u www-data app php occ config:system:set default_timezone --value=America/Maceio
```

Validacao:

```bash
docker compose exec -T -u www-data app php occ config:system:get default_language
docker compose exec -T -u www-data app php occ config:system:get default_locale
docker compose exec -T -u www-data app php occ config:system:get default_phone_region
docker compose exec -T -u www-data app php occ config:system:get default_timezone
```

## Referencias

```text
decision-records/0017-application-regional-defaults.md
standards/DOCKER_STANDARD.md
```
