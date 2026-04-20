# LoL Draft Assistant — pour lolmanager.com

> ⚠️ **Ce projet est spécifiquement conçu pour le jeu [lolmanager.com](https://lolmanager.com) de Night.**
> Les données de counterpicks, synergies et matchups proviennent directement de ce jeu et ne sont PAS représentatives du vrai League of Legends ni d'aucun autre jeu. Cet outil n'a aucune utilité en dehors de lolmanager.com.

Outil desktop pour optimiser tes drafts dans le manager de LoL par Night. Interface inspirée du client officiel LoL, avec les 170 champions, 2464 counterpicks et 692 synergies spécifiques au jeu.

## Aperçu

L'outil t'aide à :
- Évaluer en temps réel les matchups de lane et la synergie d'équipe selon les données de lolmanager.com
- Recommander les meilleurs picks par rôle selon la compo adverse et tes alliés déjà lockés
- Suggérer les bans les plus menaçants
- Calculer automatiquement la draft optimale avec un algorithme de recuit simulé (Simulated Annealing)

## Fonctionnalités

- **170 champions** avec leurs rôles selon lolmanager.com
- **2464 counterpicks** asymétriques (A counter B ≠ B counter A)
- **692 synergies** entre alliés
- **Undo/Redo** (Ctrl+Z / Ctrl+Y) avec historique de 50 actions
- **Filtrage intelligent** : les champions bannis ou déjà pickés disparaissent des recommandations
- **Mise à jour en temps réel** de la draft optimale à chaque pick/ban
- **Menu contextuel** pour assigner les champions flex (Top/Jungle, Mid/Bot, etc.) au rôle voulu
- **Phases de draft claires** : boutons visuels distincts pour pick allié, pick ennemi, ban allié, ban ennemi
- **Portraits officiels** téléchargés depuis Data Dragon (CDN de Riot) pour l'affichage

## ⚠️ Important — Portée du projet

Les données embarquées dans ce script (counterpicks, synergies, rôles) proviennent **exclusivement du jeu lolmanager.com de Night**. Elles ne correspondent pas :
- Aux matchups réels de League of Legends sur Summoner's Rift
- Aux tier lists ou guides externes (u.gg, op.gg, lolalytics, etc.)
- À aucune autre meta ou version du jeu

Utiliser cet outil pour drafter dans le vrai LoL n'a donc aucun sens. Il est conçu et calibré uniquement pour optimiser les drafts dans le jeu manager.

## Installation

### Option 1 : Lancer le script Python

Prérequis : Python 3.8+ avec Tkinter (inclus par défaut sous Windows).

```bash
pip install Pillow
python lol_draft.py
```

Au premier lancement, l'app télécharge les portraits des champions (~3 Mo) dans `~/.lol_draft_cache/`. Les lancements suivants utilisent le cache local.

### Option 2 : Compiler en exécutable Windows

```bash
pip install pyinstaller pillow
pyinstaller --onefile --windowed --name "LoLDraftAssistant" lol_draft.py
```

L'exécutable sera dans `dist/LoLDraftAssistant.exe`.

## Utilisation

1. **Sélectionne une phase** en haut à droite : 🛡 BAN ALLIÉ, ✓ PICK ALLIÉ, ⚔ PICK ENNEMI, 🛡 BAN ENNEMI
2. **Clique sur un champion** dans la grille centrale :
   - Si le champion n'a qu'un rôle possible, il se place directement au bon endroit
   - Si le champion est flex (ex. Sett Top/Support), un menu te demande à quel rôle l'assigner
3. **Clique sur un slot rempli** pour le vider
4. **UNDO / REDO** pour annuler/refaire (ou Ctrl+Z / Ctrl+Y)
5. **APPLIQUER** dans le footer pour valider la draft optimale suggérée

## Comment le modèle fonctionne

Le score d'une draft combine :
- Matchups directs en lane (×1.3)
- Matchups croisés (jungle/support vs autres lanes, ×0.7)
- Synergies entre alliés (×1.8)
- Pénalité des synergies ennemies contre tes champions (×0.4)
- Pénalité de faiblesse : -4 si une lane est à -3 ou pire

L'optimiseur utilise le recuit simulé (Simulated Annealing) avec 3000-5000 itérations pour explorer les millions de combinaisons possibles tout en respectant tes picks verrouillés, les bans et les champions adverses.

## Structure des données

Les données (issues de lolmanager.com) sont embarquées dans le script :
- `CHAMPS_ROLES` : dict `{nom: [rôles]}`
- `COUNTERS` : dict `{a: {b: force}}` pour "a counter b"
- `SYNERGIES` : dict `{a: {b: force}}` symétrique
- `DISPLAY_TO_INTERNAL` / `INTERNAL_TO_DISPLAY` : alias pour l'affichage (K'Sante ↔ KSante, Wukong ↔ MonkeyKing, etc.)

## Technologies

- Python 3 + Tkinter pour l'interface
- Pillow (PIL) pour le traitement des portraits
- Data Dragon (Riot Games) pour les portraits des champions
- PyInstaller pour la compilation en .exe

## Licence

Projet personnel non affilié à Night ni à lolmanager.com. League of Legends et ses assets appartiennent à Riot Games. lolmanager.com est un jeu indépendant de Night.
