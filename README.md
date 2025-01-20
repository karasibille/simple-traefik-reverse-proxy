# Traefik

## Créer le network pour Traefik

`docker network create traefik_web`

## Lancer Traefik
`docker-compose up -d`

### Accès au dashboard : 
http://dashboard.traefik.me/dashboard/

---

## Adapter un projet pour le rendre compatible traefik

- ajouter le network traefik_web (*docker-compose.yaml* de votre projet)
```yml
networks:
    traefik_web:
        external: true
```

- ajouter à vos services le network *traefik_web* (*docker-compose.yaml* de votre projet)
```yml
services:
    votre_service:
        networks:
            - ...
            - traefik_web
```

- ajouter à vos services les labels traefik (*docker-compose.yaml* de votre projet)
```yml
services:
    votre_service:
        labels:
            - "traefik.enable=true"
            - "traefik.docker.network=traefik_web"
            # <votre_service> ici doit être unique pour eviter les conflicts
            - "traefik.http.routers.<votre_service>.rule=Host(`${PROJECT}.traefik.me `)"
            # fonctionne aussi avec les url en .dev.test ou traefik.me 
```
