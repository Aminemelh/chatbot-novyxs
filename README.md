# Novyxs — Chatbot IA pour PME (étude de cas)

> Ce dépôt explique **comment** j'ai conçu et je fais rouler Novyxs : l'architecture, les technologies et les contraintes.
> Le code source reste privé : c'est un produit commercial en exploitation.

🌐 [novyxs.com](https://novyxs.com)

## Le problème

Les petites entreprises (ateliers, boutiques, commerces locaux) reçoivent toujours les mêmes questions : heures, prix, délais, contact. Elles n'ont ni le temps ni le budget d'avoir quelqu'un qui répond en continu.

## La solution

Un chatbot IA installé sur le site du client. Il répond **uniquement à partir des informations de l'entreprise** et redirige vers un humain quand il ne sait pas. Vendu en **abonnement mensuel (SaaS)**.

## Comment ça marche

```
Site du client ──► Crawl ──► Découpage en "chunks" ──► Base de connaissances
                                                          │
Question du visiteur ──► Recherche des chunks pertinents ─┘
                                   │
                                   ▼
                     LLM (Gemini) + contexte ──► Réponse
                                   │
                              Cache des réponses fréquentes
```

1. **Analyse du site** : je récupère le contenu du site du client, puis je le structure en une base de connaissances validée, avec des champs « officiels » : heures, prix, services, politiques.
2. **Recherche (RAG)** : à chaque question, les passages les plus pertinents sont choisis et notés selon leur pertinence.
3. **Génération** : le modèle répond seulement avec ces passages. S'il n'a pas l'info, il le dit et donne le contact de l'entreprise, au lieu d'inventer.
4. **Cache** : les questions fréquentes reçoivent une réponse instantanée, ce qui réduit aussi les coûts d'API.

👉 Une démo interactive du pipeline est sur mon [portfolio](https://aminemelh.github.io/portfolio-amine/).

## Stack

| Couche | Technologies |
|---|---|
| Front / widget | JavaScript, HTML/CSS |
| Backend | Node.js |
| Données | Supabase (PostgreSQL) |
| IA | Gemini, RAG |
| Hébergement | Vercel |
| Infra / domaine | DNS (Squarespace → Vercel), courriel pro Zoho Mail |

## Contraintes et décisions

- **Zéro invention (hallucinations)** : les réponses sont limitées à la base de connaissances. Une réponse du genre « je ne sais pas, appelez-nous » vaut mieux qu'une fausse information sur un prix.
- **Coûts** : en SaaS pour PME, chaque appel d'API compte. Le cache et la sélection de quelques passages seulement gardent les coûts bas.
- **Plusieurs clients** : les données de chaque client sont séparées. Les environnements pro et perso sont aussi isolés, avec des comptes Supabase distincts.
- **Coûts d'opération** : j'ai migré le courriel de Google Workspace vers Zoho Mail pour réduire les frais fixes.
- **Mise en place des clients** : le processus d'analyse de site et de base de connaissances est standardisé pour être répétable.

## Résultats

- Chatbots déployés pour des clients réels : un atelier de réparation de petits moteurs et un détaillant de cartes sportives, sur la Rive-Sud de Montréal.
- Gestion complète, seul : produit, technique, infrastructure, ventes et support client.

## Ce que j'ai appris

Concevoir un pipeline RAG de bout en bout, gérer un DNS et l'hébergement en production, penser coûts et fiabilité pour de vrais clients, et parler technique à des gens qui ne sont pas techniques.

---
*Amine Meliouh — Montréal · [Portfolio](https://aminemelh.github.io/portfolio-amine/) · [LinkedIn](https://www.linkedin.com/in/amine-meliouh-31a32427a)*
