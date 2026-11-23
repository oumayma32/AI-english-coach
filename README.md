# AI Interview Coach

Entretiens d'embauche simulés en anglais avec un recruteur IA vocal, puis rapport détaillé.

## Lancer
1. Node.js 20.6 ou plus récent.
2. `npm install`
3. `cp .env.example .env` puis renseigner `GEMINI_API_KEY`.
4. `npm start` puis ouvrir http://localhost:3000 (Chrome ou Edge pour la reconnaissance vocale).

## Architecture
- `server.js` : API Express (setup, tours de conversation, rapport JSON, historique, suppression), appels à l'API Gemini côté serveur uniquement (la clé n'est jamais exposée), validation des entrées, limitation de débit, en-têtes de sécurité, stockage JSON dans `data/`.
- `public/index.html` : interface (setup, entretien en direct, rapport, historique). Reconnaissance vocale et synthèse vocale via les API du navigateur (Web Speech), mode texte accessible.

## Choix MVP et évolutions
- Mode tour par tour (enregistrer, vérifier, envoyer), comme recommandé par le brief.
- Pas de comptes : session démo. Pour la production : authentification, PostgreSQL/MySQL à la place du fichier JSON, STT/TTS serveur (Whisper, ElevenLabs...) pour une meilleure prononciation.
- Prononciation, fluidité et confiance sont des estimations de coaching basées sur le texte et la confiance de reconnaissance, jamais un test certifié.

## Correction immédiate
Après chaque réponse, le coach (Gemini) renvoie en parallèle de la question suivante : score, erreurs surlignées, version corrigée, explications, conseil. L'utilisateur peut écouter la version corrigée puis la redire (micro ou clavier) ; un pourcentage de correspondance lui indique s'il l'a bien reformulée.

## Héberger en ligne (Render, gratuit)
1. Mettre le dossier sur GitHub (le fichier `.env` est ignoré, la clé n'est jamais publiée).
2. render.com > New > Web Service > choisir le dépôt. Build: `npm install`, Start: `npm start`, plan Free.
3. Dans Environment, ajouter `GEMINI_API_KEY` (et `MODEL=gemini-2.5-flash`).
4. Le lien `https://...onrender.com` est en HTTPS (obligatoire pour le micro).
Chaque navigateur ne voit que ses propres sessions. Le plan gratuit s'endort après 15 minutes et perd le fichier `data/` à chaque redémarrage.
