# Pause AR — contexte du projet

Le projet s'appelle **Pause AR**, descriptif : *« Chaque semaine, l'essentiel des publications qui
comptent en anesthésie-réanimation : essais pivots, recommandations et grandes méta-analyses. »*
Le fichier principal est **`index.html`**, publié automatiquement par GitHub Pages sur
**https://pausear.fr/** (nom de domaine OVH branché le 11/09/2026 ; `www.pausear.fr` y renvoie ; le
dépôt garde le nom `PAUSE-AR`). C'est un tableau de bord de veille scientifique en
anesthésie-réanimation.

**Origine (10/09/2026)** : Pause AR est la copie conforme de **Pause Cardio**
(dépôt `hacuubo/veille-cardio`, https://pausecardio.fr), projet terminé de veille en cardiologie.
Toute l'architecture, la présentation, les outils, la routine quotidienne et la chaîne qualité en sont
repris tels quels ; **chaque règle décidée ou corrigée sur Pause Cardio entre le 19/08/2026 et le
09/09/2026 est reprise ici, reformulée pour l'anesthésie-réanimation, aucune n'a été retirée.** Seuls
changent : le domaine médical (surspécialités, revues, congrès, mots-clés), la couleur de marque et le
tracé du logo. Quand une règle de ce fichier porte une date, c'est la date de la décision d'origine ;
toutes ont été adoptées pour Pause AR le 10/09/2026.

Ambition à moyen terme : devenir une source reconnue de l'anesthésie-réanimation francophone et
collecter des inscriptions au bulletin. La feuille de route en cinq phases est la même que celle de
Pause Cardio (site → audience → application web installable → stores) ; l'app native n'est pas la
première étape.

## À qui ça sert

Anesthésistes-réanimateurs et médecins francophones. Le site permet de retrouver en un coup d'œil les
sorties récentes susceptibles de changer la pratique quotidienne — au bloc, en réanimation, en salle de
surveillance post-interventionnelle. Il est consulté sur téléphone, entre deux interventions.
Interlocuteur non développeur : explique les choses simplement, et évite le jargon technique.

## Ce que contient la page

Structure : sections par **surspécialité** — **rangées automatiquement par le script de la plus fournie à
la moins fournie** sur l'année cochée (compte des cartes, ordre stable en cas d'égalité, décision du
03/09/2026 : l'ordre des sections dans le HTML n'a plus d'importance) → sous-groupes par **année** (année
en cours d'abord, puis N−1) → articles triés par **date de parution, les derniers parus en tête**
(décision du 29/08/2026) : insérer toute nouvelle carte en haut de son année, et lire la date dans la
ligne `.meta` (jour facultatif, mois en toutes lettres, année). Les cartes sans mois lisible restent en
fin d'année. Chaque surspécialité est un **tiroir replié** : au chargement, la page n'affiche que la
liste des dix titres avec leur nombre de sorties ; un clic sur un titre ouvre ses articles (années
comprises), un second le referme, et **un seul tiroir est ouvert à la fois** — en ouvrir un referme le
précédent (décision du 31/08/2026). Même exclusivité pour les fiches : déplier un article referme celui
qui était déplié.

- Surspécialités (`data-spec`, arrêtées le 10/09/2026) :
  - `periop` (Anesthésie périopératoire — évaluation et risque, hémodynamique peropératoire, monitorage,
    complications postopératoires, patient cardiaque au bloc) ;
  - `alr` (Anesthésie locorégionale & douleur — blocs, rachianesthésie, douleur post-opératoire,
    opioïdes) ;
  - `vent` (Ventilation, SDRA & oxygénation — ventilation protectrice, intubation, sevrage, ECMO,
    oxygène) ;
  - `sepsis` (Sepsis & infections graves — antibiothérapie, choc septique, corticoïdes, vasopresseurs) ;
  - `hemo` (Hémodynamique, choc & remplissage — fluides, transfusion, hémostase, assistance) ;
  - `neurotrauma` (Neuroréanimation & traumatologie — traumatisme crânien, hémorragie méningée,
    polytraumatisé, transfusion massive) ;
  - `obst` (Anesthésie obstétricale) ;
  - `ped` (Anesthésie-réanimation pédiatrique) ;
  - `arret` (Arrêt cardiaque, urgences & pré-hospitalier — réanimation cardio-pulmonaire, contrôle
    ciblé de la température, défibrillation, ECPR, prise en charge pré-hospitalière) ;
  - `sedation` (Sédation, delirium & après-réanimation — sédation et analgésie en réanimation,
    delirium, sommeil, mobilisation précoce, neuromyopathie de réanimation, devenir à distance et
    syndrome post-réanimation).

  Ce dixième tiroir a été détaché d'`arret` le 11/09/2026 : la sédation et l'après-réanimation
  produisent assez d'essais pour vivre seules, et « arrêt cardiaque » gagne à ne parler que de l'arrêt.

  En créer une nouvelle si besoin avec sa couleur (`--series-N`), sa section, **et son entrée dans les
  cinq tables jumelles** : `SPECS` du script en bas d'`index.html`, `SPECS` d'`outils/bulletin.mjs`,
  `SPECS` d'`outils/pages-articles.mjs`, `SPECS` d'`outils/moisson.mjs`, et la liste `SPECS` d'en-tête
  d'`outils/controle-cartes.mjs`. Ne jamais en changer une sans les autres.
- Il ne reste **que deux filtres** : les années et la recherche. Les puces de surspécialité et de niveau
  ont été retirées le 24/08/2026 — le sommaire replié fait le tri, et le niveau se lit sur chaque ligne.
  Ne pas les réintroduire sans demande explicite.
- Années : deux puces — **seule l'année en cours est cochée par défaut** (décision du 29/08/2026),
  le lecteur coche l'année précédente s'il veut l'an dernier (ce ne sont pas des boutons exclusifs). Si
  les deux sont décochées, la page affiche le message « Aucune année sélectionnée ». Les puces et les
  en-têtes d'année ne portent que le millésime (pas de mention « en cours »). **Les deux puces sont
  fabriquées par le script à partir de la date du jour** (année en cours, année précédente ; constantes
  `ANNEE` et `ANNEES`, décision du 08/09/2026), et les cartes plus anciennes que l'an dernier sont
  **retirées de la page au chargement**, purement et simplement (compteurs, bandeau et favoris compris).
  Au changement d'année il n'y a donc rien à modifier dans le HTML : le premier samedi de janvier, la
  routine retire seulement les vieux groupes d'année du HTML pour alléger la page (ligne `A_FAIRE` de
  `congres.mjs`) et crée le groupe de la nouvelle année dans une surspécialité au moment d'y insérer sa
  première carte.
- Niveaux (`data-lvl`) : `crit` / `warn` / `watch`. Depuis le 29/08/2026 ils **ne s'affichent plus sur la
  plateforme** (badges masqués en CSS) mais restent obligatoires sur chaque carte : le bulletin et le
  courriel s'en servent pour leur tri, et ils gardent le classement éditorial. La seule mise en avant
  visuelle est réservée aux **recommandations** : le script pose la classe `reco` (fond légèrement teinté
  de la couleur de marque) sur toute carte dont le `span.type` contient « Recommandation ». La pastille
  verte « nouveau » est conservée : depuis le 09/09/2026 elle marque les cartes du **dernier courriel
  envoyé** (`bulletin/semaine.json`), et non plus une fenêtre glissante de 7 jours (règle gardée en
  secours seulement).
- Chaque carte porte `data-kw` : mots-clés **bilingues FR + EN** avec les acronymes de la spécialité dans
  les deux langues (SDRA/ARDS, ALR, PEP/PEEP, ECMO, RCP/CPR, AG, TC/TBI, VNI/NIV, IRA/AKI…), qui
  alimentent la barre de recherche.

## Règles éditoriales

- **Charte de rédaction** (`outils/CHARTE-REDACTION.md`, adoptée le 05/09/2026) : tout texte français —
  accroche, résumé, résultat principal, fiche, « En pratique », courriel — doit se lire comme écrit
  directement en français par un médecin habitué à la synthèse scientifique. Pas de calque de
  l'anglais ni de style télégraphique, phrases complètes avec sujet, une idée par phrase, terminologie
  française usuelle, abréviations peu courantes développées, degré d'affirmation calé sur le niveau de
  preuve, distinction association/causalité, critère principal/secondaire, relatif/absolu. Le résumé
  français existant n'est pas une référence : on vérifie sur le résumé PubMed. Toute nouvelle carte
  suit `outils/BRIEF-CARTE.md` (format) et la charte (fond), puis passe par une **relecture de
  fidélité distincte** avant publication. Les révisions gardent l'ancienne version et un journal dans
  `revision/AAAA-MM-JJ/`.
- **Titres des articles dans leur langue d'origine** (anglais si l'article est anglais).
  Tout le reste — résumés, fiches, interface — en français.
- Sélection stricte : essais randomisés pivots, recommandations, méta-analyses majeures. Pas de cohortes
  anecdotiques. Mieux vaut 5 sorties qui comptent que 20 sans intérêt.
- Chaque entrée a un **lien vers l'article original** (NEJM, PubMed, JAMA, ICM, BJA…) et, si possible, un
  lien d'analyse (site de la société savante, relais francophone).
- Fiche de lecture sur **chaque carte, tous niveaux confondus** (décision du 30/08/2026) : question
  clinique → méthode → résultats chiffrés (HR, IC95 %, NNT) → limites → **« En pratique »** (ce que ça
  change au quotidien), suivie de la mention « Résumé à valider par le lecteur avant application
  clinique — se reporter à l'article original en lien » (depuis le 29/08/2026, aucune mention d'auteur
  dans les fiches ni les bulletins).
- Ne jamais inventer un chiffre, un titre ou un lien : vérifier par recherche web, sinon omettre.
- **Aucune référence géographique ou personnelle** dans les résumés et fiches (pas de « notre service »,
  « notre réanimation », « en France » comme contexte de pratique) : le contenu doit servir n'importe
  quel anesthésiste-réanimateur francophone. Aucune mention d'auteur ni de Claude — seulement
  « Fiches rédigées à l'aide de l'IA » dans les pieds de page.
- **Grands congrès** (SFAR, ESAIC, ESICM…) : couverture **exhaustive des recommandations et documents de
  consensus** présentés — les vérifier une à une sur PubMed et le site du congrès, aucune ne doit
  manquer. Les sorties de congrès restent en ligne **au moins trois mois** après la fin du congrès ;
  quand la publication définitive paraît, mettre la carte à jour plutôt que la retirer.

## Chaîne qualité — obligatoire pour tout texte diffusé (règle du 06/09/2026)

Cinq pièces, toutes versionnées dans le dépôt, s'appliquent à **chaque nouvelle carte, chaque mise à
jour de carte et chaque courriel** ; la routine, les sous-agents et toute session qui touche au contenu
doivent les suivre, sans exception :

1. **`outils/CHARTE-REDACTION.md`** — le fond : ton de médecin francophone, langue, fidélité
   scientifique, rubrique « En pratique », méthode en deux passes.
2. **`outils/BRIEF-CARTE.md`** (nouvelle carte) et **`outils/BRIEF-REVISION.md`** (carte existante) — le
   format et la livraison ; le rédacteur part du résumé PubMed (`efetch`) et des pages ouvertes, jamais
   de mémoire.
3. **`outils/BRIEF-CONTROLE.md`** — la relecture de fidélité par un **agent distinct du rédacteur** :
   chiffres, groupes, critères, degré de certitude, cohérence accroche/résumé/résultat principal/fiche.
   Aucune carte n'est insérée dans `index.html` sans cette relecture.
4. **`outils/BRIEF-LANGUE.md`** — la relecture de langue par un **troisième agent**, distinct du
   rédacteur ET du relecteur de fidélité (règle du 13/09/2026). On ne lui donne **pas** la source :
   son travail n'est pas de vérifier, c'est d'entendre. Il lit en anesthésiste-réanimateur
   francophone, chaque phrase à voix haute, et ne corrige que la langue — calques de l'anglais, style
   télégraphique, phrases qui se démontent, abréviations, ton, typographie, musique d'ensemble entre
   l'accroche, le résumé, le résultat principal et la fiche. **Il ne touche à aucun chiffre ni à aucun
   fait** : une phrase qui lui paraît fausse est signalée dans `doutes_de_fond` et repart au relecteur
   de fidélité, jamais tranchée par lui.
   Pourquoi une passe séparée : sur Pause Cardio la langue n'était que le quatrième critère d'une
   relecture scientifique, et des tournures maladroites sont passées — exactes, mais pénibles à lire.
   Un relecteur qui vérifie des chiffres lit pour contrôler, pas pour entendre.
5. **`node outils/controle-cartes.mjs`** — le contrôle automatique, à lancer après toute modification :
   structure, accroche, date de parution, typographie française, formules interdites (« vs », « Au
   cabinet », mention d'auteur ou d'IA, référence géographique, effets journalistiques), cohérence des
   chiffres entre les présentations ; avec `--sources`, fidélité des chiffres au résumé PubMed. Par
   défaut il contrôle les cartes ajoutées ou parues depuis 8 jours ; `--tout` couvre le site. Avec
   `--strict`, une ERREUR bloque. **`outils/faire-bulletin.sh` l'exécute en mode strict avant tout
   bulletin, et le workflow `bulletin-inscrits.yml` avant tout envoi Brevo : un courriel ne part pas
   tant qu'une carte de la semaine est en ERREUR.** Les AVERTISSEMENTS ne bloquent pas mais doivent
   être traités par la routine quand ils concernent une carte de la semaine.

Révision par lots (plusieurs cartes anciennes) : `outils/revision/extraire.py` inventorie et conserve
les versions en cours dans `revision/AAAA-MM-JJ/avant/`, les lots passent par BRIEF-REVISION puis
BRIEF-CONTROLE, et `outils/revision/appliquer.py` applique les textes validés avec le garde-fou sur les
chiffres et écrit `revision/AAAA-MM-JJ/journal.md` (corrections sourcées, passages à vérifier).

## Présentation (ne pas casser)

La page affiche chaque article sur **une ligne repliée** : titre d'origine, accroche française en dessous,
puis une ligne de repère *revue · date · type d'étude*. Un clic déplie la fiche complète, un second replie
(le dépliage est animé). **Cette mise en forme est construite automatiquement par le script en bas de
`index.html`** à partir du HTML des cartes : écris donc les cartes au format long habituel
(`<article class="card">` avec `.top`, `<h3>`, `.meta`, `.sum`, `.actions`, `.fiche`) et l'accordéon se
fabrique tout seul. Ne pas écrire de cartes « déjà compactes ».

Trois attributs et un bloc à renseigner **sur chaque nouvelle carte** :

- `data-ajout="AAAA-MM-JJ"` — **obligatoire** : date d'ajout, gardée pour la traçabilité des lots.
  Depuis le 01/09/2026 elle ne pilote plus l'affichage : la pastille verte « nouveau » et le bandeau
  « Cette semaine » sont déclenchés par la **date de parution** lue dans la ligne `.meta`
  (« *Revue* · 5 août 2026 · … »), pendant 7 jours après parution — un article repris tardivement
  n'est donc plus marqué « nouveau » à tort. Le **jour** doit figurer dans `.meta` pour qu'une carte
  soit signalée (sans jour, pas de pastille) : toujours l'écrire pour les sorties de la semaine.
- `data-fr="…"` — accroche française de six à dix mots, **obligatoire sur chaque carte, quel que soit
  le niveau** : c'est la ligne que le lecteur lit sous le titre anglais, et celle qui s'affiche dans le
  bandeau « Cette semaine ». Elle doit se lire à voix haute sans buter : une phrase de français courant,
  verbe conjugué, jamais un chiffre laissé en suspens en fin de phrase (écrire « chez l'opéré fragile,
  une hypotension sur quatre passe inaperçue », pas « … passe inaperçue sur quatre »).
  Développer les sigles peu courants, garder ceux que tout anesthésiste-réanimateur lit d'un coup d'œil
  (SDRA, PEP, ALR, AG, ECMO). Espace insécable avant `:`, `?` et `%`.
- `<div class="cle">…</div>` — **juste après `<p class="sum">`**, hors de la fiche : le chiffre clé de
  l'étude, repris mot pour mot de la fiche (HR, IC95 %, pourcentages, avec les nombres en `<b>`). Le script
  l'affiche en bandeau « Résultat principal ». À omettre pour les recommandations sans chiffre unique.
- Le bloc `.verdict` (« En pratique ») reste écrit à sa place habituelle dans la fiche : **le script le
  remonte tout seul** juste sous le résumé, avec un libellé en capitales. Ne pas le déplacer à la main.

Autres règles de mise en page :

- En-tête (`header.site-head`) : **rien au-dessus du logo**, et tout est **centré**. Dans l'ordre — le nom
  **PAUSE AR** (`h1.brand-name`) au milieu, avec la marque de capnographie (`svg.brand-mark`) à sa droite
  sur ordinateur et **sous le nom sur téléphone** (moins de 560 px) ; la ligne discrète `.eyebrow` (nombre
  de sorties + date de mise à jour — **recalculés automatiquement** par le script depuis les cartes :
  compte des `article.card`, date du `data-ajout` le plus récent ; ne plus les écrire à la main, le
  `span#maj-ligne` n'est qu'un texte de secours) ; le liséré de marque `.brand-rule` ; puis la
  seule ligne de descriptif (la même phrase que sur l'écran d'ouverture). Pas de tuiles de statistiques.
- Sous le descriptif viennent **l'encart d'inscription replié**, la ligne repliée **« Ajouter l'appli
  Pause AR »**, puis le bandeau **« Cette semaine »**. **Depuis le 09/09/2026 il rappelle exactement
  la liste du dernier courriel envoyé** : `outils/bulletin.mjs` écrit `bulletin/semaine.json` (sujet,
  lundi de la semaine, ancres des articles dans l'ordre du courriel) chaque fois qu'un courriel part
  (`--rappel` le samedi, `--congres` pour un récapitulatif ; jamais les matins de congrès), et le
  script de la page le lit. Le bandeau change donc le samedi, après la routine, et reste identique
  toute la semaine — **pas de semaine glissante**. Titre « Cette semaine · 12 sorties · semaine du
  lundi 7 septembre », « Semaine calme · rappel · … » ou « Récapitulatif SFAR 2026 · … ». La **pastille
  verte « nouveau » suit la même liste**. En secours seulement (page ouverte en local, fichier
  illisible), la règle précédente reste : cartes parues depuis moins de 7 jours d'après la ligne `.meta`.
  `node outils/bulletin.mjs --semaine-seule` réécrit le fichier depuis la mémoire du dernier lot.
  Le bandeau est **replié par défaut**, une seule ligne avec un chevron ; un clic déplie la liste, un
  second la replie. Les lignes dépliées sont cliquables : elles ouvrent le tiroir de l'article et le
  déplient. **Puis** les filtres — années et recherche. Rien d'autre, et pas de tuiles de statistiques.
- **Encart d'inscription au bulletin** : construit par le script, en deux exemplaires bâtis par la même
  fonction — une ligne repliée « ✉ Recevoir par mail chaque semaine les dernières sorties, c'est ici. »
  (libellé arrêté le 01/09/2026) sous le bandeau « Cette semaine », et la version
  dépliée `.abo.plein` juste avant le pied de page. Trois règles : l'adresse du formulaire Brevo se met
  **uniquement** dans la constante `ABO_FORM` en bas du script ; **tant qu'elle est vide, l'encart n'est
  pas affiché du tout** (pas de champ qui ne mène nulle part) ; l'envoi vise une fenêtre invisible
  (`iframe[name=pa-abo-cadre]`) pour que le lecteur ne quitte pas la page. On ne peut donc pas lire le
  verdict de Brevo : c'est le mail de confirmation (double opt-in) qui fait foi, et le message affiché
  le dit ainsi. Ne pas retirer la case à cocher de consentement ni la mention sur l'usage de l'adresse.
  **Jamais de reCAPTCHA sur le formulaire Brevo** (règle du 12/09/2026) : la page n'affiche pas le
  formulaire hébergé par Brevo, elle poste ses propres champs (`EMAIL`, `locale`, `html_type`, et le
  piège à robots `email_address_check`) sur l'adresse `serve/`. Un captcha attendrait un jeton que la
  page ne peut pas fournir et Brevo refuserait **chaque** inscription, sans erreur visible ; Brevo le
  recommande pourtant d'un bandeau à la création du formulaire. Le champ invisible suffit. Côté
  réglages du formulaire : double confirmation obligatoire, et les deux « pages de confirmation » de
  Brevo restent décochées — la page affiche son propre message après l'envoi, et Brevo montre sa page
  standard après le clic sur le lien.
- **Encart « Ajouter l'appli Pause AR »** (`section.instal`, règle du 03/09/2026) : construit par le
  script (bloc « installation sur l'écran d'accueil » en bas de page), en deux exemplaires comme
  l'inscription — une ligne repliée « 📱 Ajouter l'appli Pause AR » **juste sous l'encart d'inscription
  replié**, et la version dépliée `.instal.plein` après l'inscription dépliée, avant « Rythme de mise à
  jour ». Pas de paragraphe d'introduction : le contenu commence par les deux onglets « iPhone · iPad » /
  « Android », l'appareil détecté étant présélectionné. Sur iPhone : les trois étapes « ⋯ » (à droite de
  la barre d'adresse, depuis iOS 26) → Partager → « Sur l'écran d'accueil » → « Ajouter », et rien
  d'autre — Apple n'autorise aucun site à déclencher l'installation, et la feuille de partage ouverte par
  `navigator.share` **ne contient pas** « Sur l'écran d'accueil » (bouton essayé puis retiré le
  03/09/2026 : ne pas le remettre). Sur Android : quand Chrome propose l'installation
  (`beforeinstallprompt`), **un appui sur la ligne repliée lance directement la fenêtre d'installation**,
  sans rien déplier ; sinon la ligne se déplie sur le bouton « Installer Pause AR » (caché tant que
  Chrome ne le permet pas) et les étapes menu ⋮ → « Ajouter à l'écran d'accueil ». Trois règles :
  **il n'est jamais affiché quand la page est déjà ouverte depuis l'icône** (mode `standalone`), il
  reste en teinte neutre (pas la teinte de marque, réservée à l'inscription et aux recommandations), et
  il disparaît pendant une recherche comme les autres encarts.
- **Encart « Radar — les prochaines semaines »** (design « fil chronologique » choisi le 02/09/2026) :
  en bas de page, **une seule carte**, une liste `ol#radar-fil` où chaque `<li>` porte l'échéance
  (`<span class="q">`) puis le libellé (`<span class="t">`, nom en `<b>`). Deux sortes de lignes :
  - `class="pub"` (point bleu) : **publications attendues** — essais présentés en attente de parution,
    recommandations annoncées. **C'est la seule partie que la routine entretient**, à chaque veille
    du samedi et au lendemain de chaque congrès : retirer ce qui est paru, ajouter ce qui s'annonce,
    dates vérifiées par recherche web, jamais inventées.
  - congrès (point de marque = niveau 1, gris = niveau 2) : **ajoutés automatiquement par le script**
    depuis `outils/congres.json` (les 4 prochains dans les 150 jours, « En cours » pendant le congrès,
    « à confirmer » si la date n'est pas confirmée). Ne jamais écrire de congrès à la main : pour
    en changer, corriger le calendrier. Les `<li class="secours">` ne servent que si le calendrier
    est illisible (retirés sinon) — les tenir à jour à l'occasion, sans plus.
  Ne pas y remettre de deuxième carte « publié à … » : ce qui est paru va dans les sections.
- **Encart « Rythme de mise à jour »** (`section.rythme`, règle du 01/09/2026) : juste avant le pied
  de page, après l'encart d'inscription déplié. Texte fixe et discret qui explique le fonctionnement :
  samedi matin (veille + bulletin 8 h, même semaine calme), mise à jour quotidienne pendant SFAR, ESAIC
  et ESICM avec récapitulatif le lendemain de la clôture, autres congrès repris le samedi. À modifier
  seulement si le rythme change.
- **Couleurs** : la surspécialité ne sert plus que de fin liséré à gauche de la carte (et de couleur du
  libellé « En pratique ») ; le fond légèrement teinté de marque est réservé aux cartes `reco`
  (recommandations). Ne pas remettre de grosse pastille de couleur ni de badge de niveau par article.
- **Favoris et articles lus** (règle du 06/09/2026) : construits par le script, mémorisés **sur
  l'appareil seulement** (`localStorage`, clés `pa-favoris` et `pa-lus`, listes d'ancres de cartes — la
  même ancre que les liens du bulletin, donc stable). Une étoile (`button.fav-btn`, hors du bouton de
  ligne) à droite de chaque ligne, précédée d'une **coche « lu »** (`button.lu-chk`, cercle vide puis
  plein) : c'est le lecteur qui décide, **déplier une fiche ne marque rien** ; la carte cochée passe en
  `lu` (titre et accroche grisés), un second appui l'annule. **Trois listes exclusives** : la liste
  principale ne montre que les articles **ni lus ni favoris** — cocher ou étoiler un article le fait
  quitter la liste aussitôt ; les puces « ★ Mes favoris · N » et « ✓ Lus · N » de la rangée « Lecture »
  affichent chacune les leurs, **toutes surspécialités et années confondues**, tiroirs ouverts, encarts
  masqués comme lors d'une recherche (`state.vue` : `tout` / `favoris` / `lus`, `choisirVue()`). Un lien
  direct vers une carte (bulletin, bandeau) bascule sur la liste qui la contient. Pas de recherche dédiée
  aux favoris. Rien ne quitte l'appareil, aucun compte.
- **Bouton « Partager »** (règle du 08/09/2026) : posé par le script en fin de rangée `.actions` de
  chaque fiche, jamais écrit dans le HTML des cartes. Sur téléphone il ouvre la feuille de partage du
  système (WhatsApp, SMS, mail) avec le titre d'origine, l'accroche française et le lien direct
  `https://pausear.fr/#ancre` (constante `SITE_URL` du script, la même adresse que dans
  les courriels) ; ailleurs il copie le lien et affiche « Lien copié » (`#toast`).
- **Tiroirs de surspécialité** : la tête de section (`.spec-h`) est cliquable (chevron à droite, `role`
  et `aria-expanded` posés par le script) ; l'état ouvert est porté par `section.spec.ouvert` et par
  l'ensemble `ouverts` du script. Trois règles à ne pas casser : les tiroirs ouverts sont **mémorisés
  d'une visite à l'autre** (`localStorage`, clé `pa-ouverts` — un seul désormais) et rouverts au
  chargement, une **recherche ouvre tout** — sinon le lecteur ne verrait pas ses propres résultats —, et
  le compteur de la tête (`.count`) est recalculé à chaque filtrage, il ne doit plus être écrit en dur
  dans le HTML.
- **Confort de lecture** : le texte des fiches est limité à 68 caractères de large, les lignes d'articles
  font au moins 44 px de haut, et l'en-tête de surspécialité reste collé en haut pendant le défilement.
- **Écran d'ouverture** : au chargement, un plein écran dessine le logo — une courbe de capnographie qui
  se déroule, s'interrompt sur les deux barreaux bleu-vert du symbole pause, puis repart — suivi du nom
  « PAUSE AR » et du descriptif, avant de s'effacer sur l'accueil — **durée totale 5 secondes** (maintien
  puis fondu). Une ligne discrète « Touchez l'écran pour entrer » apparaît au bout de 2 secondes :
  l'écran se saute d'un clic ou d'une touche. Il ne rejoue pas dans la même session (`sessionStorage`,
  clé `pa-ouverture`) et se réduit à un bref fondu si le lecteur a demandé moins d'animations. Le
  balisage est en tête de `<body>` (`#splash`), les styles sous « écran d'ouverture ».

## Identité

Le site s'appelle **PAUSE AR**. Le logo est le symbole pause (⏸) dessiné dans une **courbe de
capnographie** : la ligne de base part à gauche, monte en pente raide (montée expiratoire), file en
plateau alvéolaire légèrement ascendant, redescend d'un trait (reprise inspiratoire), puis s'interrompt
sur les deux barreaux bleu-vert avant de recommencer un cycle à droite. C'est le pendant, pour
l'anesthésie-réanimation, du tracé ECG de Pause Cardio. Il existe en deux tailles, toutes deux en SVG
écrit à la main dans `index.html` — `svg.brand-mark` (92 × 26) en tête de page, `svg.splash-mark`
(300 × 90) sur l'écran d'ouverture. **Ne pas redessiner ces tracés** ni déplacer le bloc `#splash`, qui
doit rester juste après `<body>` avec son script inline (c'est ce qui évite le clignotement au retour).
Si un tracé change, corriger aussi `stroke-dasharray` / `stroke-dashoffset` de `.splash-mark .t1` et
`.t2` (210 aujourd'hui, la longueur approchée de chaque chemin) : c'est ce qui fait courir l'animation.
Sous 560 px de large la marque d'en-tête passe sous le nom, centrée : côte à côte, la courbe serait
comprimée et le plateau deviendrait illisible.

Les dix surspécialités se partagent `--series-1` à `--series-10` ; le rapprochement exact figure en
commentaire dans le bloc `:root` d'`index.html` et dans les tables `SPECS` des outils.

Couleurs de marque : `--brand` (**#0d6e6b** bleu-vert profond en clair, **#3aada6** en sombre) et
`--brand-line` pour le tracé, avec `--brand-doux`, `--brand-doux2` et `--brand-trait` qui en dérivent.
Ne pas les confondre avec `--status-critical` (#d03b3b), réservé au niveau « changement de pratique »
des fiches — c'est le seul rouge du site.

**Icône d'application** (choisie le 10/09/2026) : fond encre `#141412`, courbe de capnographie crème
`#f2f1ea` et deux barreaux bleu-vert `#3aada6` — c'est le logo réduit à ce qui reste lisible à 60 px.
Les fichiers sont dans `icone/` et se refabriquent depuis `icone/icone.svg` (version aux coins arrondis,
pour le favicon) : `pausear-apple.png` est **pleine page, sans coins arrondis** — iOS applique son propre
masque —, tandis que `pausear-512.png` est l'icône *maskable* Android, dont le dessin est réduit à 74 %
pour rester dans la zone que le système peut rogner. **Toujours produire ces PNG en 512 × 512** :
Chromium sans interface impose une hauteur de fenêtre minimale, si bien qu'une capture demandée en 180
ou 192 px sort comprimée vers le haut, moitié basse vide. Le vérifier avec `python3 outils/png.py
icone/pausear-apple.png` avant de publier (les bandes dessinées doivent être réparties, pas toutes en
haut) ; pour obtenir une taille plus petite sans le défaut, rendre l'image **à sa taille finale doublée**
dans une fenêtre de 360 px avec `--force-device-scale-factor=0.5`. iOS réclame un `apple-touch-icon` de
**180 × 180** et va le chercher aussi à la racine : garder `apple-touch-icon.png` et
`apple-touch-icon-precomposed.png` à côté d'`index.html`, sans quoi le raccourci d'écran d'accueil
affiche la vignette de secours d'iOS (fond noir, lettre blanche). `manifest.webmanifest` à la racine
donne le nom « Pause AR » à l'icône posée sur l'écran d'accueil. Ne pas mettre l'icône arrondie dans
l'`apple-touch-icon` : les coins apparaîtraient deux fois.

**Chemin dans le manifeste** : le site vit à la **racine** de `pausear.fr` depuis le 11/09/2026 —
`start_url`, `scope` et les `src` des icônes commencent donc par `/`, et le fichier `CNAME` à la racine
du dépôt (contenu : `pausear.fr`) est ce qui tient le nom de domaine. **Ne jamais le supprimer** : sans
lui, GitHub Pages relâche le domaine au prochain déploiement et le site retombe sur `github.io`.

**Leçon gardée de la période `github.io`** (11/09/2026) : GitHub Pages **distinguait les majuscules
des minuscules** dans le chemin, le dépôt s'appelle `PAUSE-AR` et le site avait été écrit en
minuscules partout — icônes du manifeste, `SITE_URL` du bouton « Partager », liens des courriels,
`canonical` des pages par article et `sitemap.xml` pointaient tous vers un 404, alors que la page
d'accueil s'affichait normalement. Défaut invisible à l'œil. La règle qui en reste, valable pour
tout changement d'adresse : **vérifier une icône et une page d'article, pas seulement la page
d'accueil** (`curl -o /dev/null -w '%{http_code}'`).

`icone/partage.png` (1200 × 630) est l'image de partage sur les réseaux sociaux et les messageries
(`og:image`, `twitter:image`) : fond encre, logo, nom et devise. La refabriquer si le logo change.

## Méthode de veille — ne rien laisser passer

La sélection ne part **jamais de la mémoire** : elle part d'une récolte systématique. Avant toute mise
à jour d'`index.html` :

1. `node outils/moisson.mjs` (options : `--jours=N`, `--depuis=AAAA-MM-JJ`, `--spec=vent`, `--tout`
   pour voir aussi ce qui est déjà en ligne, `--brut` pour ne rien filtrer). Le script interroge PubMed
   surspécialité par surspécialité — d'abord les grandes revues tous types confondus, puis les revues
   de surspécialité pour les seuls essais randomisés, recommandations et méta-analyses — et marque d'un
   `★` ce qui relève de ces trois catégories. Il signale ce qui est déjà sur le site (comparaison sur le
   titre, préfixe compris, pour rattraper les recommandations à sous-titre à rallonge).
2. **Passer en revue chaque ligne `NOUVEAU`**, pas seulement les `★`. Un article écarté doit l'être en
   connaissance de cause, pas par omission.
3. Compléter par ce que PubMed ne voit pas encore : communications de congrès (SFAR, ESAIC, ESICM, ASA,
   SCCM, ISICEM, ATS), communiqués topline, relais des sociétés savantes.
4. Dire dans le compte rendu combien de candidats ont été examinés et combien retenus.

Règles de classement qui évitent les oublis :

- Un **suivi à long terme d'un essai pivot** (1 an, 5 ans, extension ouverte) n'est jamais « veille » :
  au minimum « à connaître ». Le devenir fonctionnel à distance d'un essai de réanimation compte autant
  que sa mortalité à 28 jours.
- Un **essai négatif** sur un traitement en vogue vaut un essai positif : c'est ce qui évite de prescrire.
- Une **méta-analyse sur données individuelles** dans une grande revue est au moins « à connaître ».
- Dans le doute entre deux niveaux, prendre le plus haut : mieux vaut une ligne de trop qu'une sortie
  qui échappe au lecteur.

## Mise à jour

- Automatique : la **routine Claude Code « Veille AR »** s'exécute **tous les jours à 05:00 UTC**
  (elle ne fixe pas de modèle : elle tourne avec celui de la session liée). Elle tourne dans une
  **session persistante qui a le dépôt attaché** (`source_url`) **et `main` comme branche de sortie**
  (`outcome_branch`). **C'est la règle la plus importante de tout ce fichier** : sur Pause Cardio, la
  routine créée le 29/08/2026 n'avait aucun dépôt attaché, elle a « réussi » chaque matin sans rien
  publier, et le samedi 05/09 est passé sans courriel. Si on recrée la routine, il faut la lier par
  `persistent_session_id` à une session qui possède le dépôt et `main`, sinon rien ne sort.
  Son premier geste est `git reset --hard origin/main`, son dernier une vérification que le commit est
  bien sur `origin/main`. Elle commence par `node outils/congres.mjs`, qui lit le **calendrier des
  congrès** `outils/congres.json` et lui donne son mode du jour, sans interprétation :
  - `MODE SAMEDI` → veille hebdomadaire complète, mise à jour du site et du Radar, puis
    `bash outils/faire-bulletin.sh --rappel` : **le courriel du samedi part toutes les semaines sans
    exception**, à 08:00. S'il y a du nouveau, c'est « Les sorties de la semaine du lundi … » ; s'il
    n'y a rien, c'est « Semaine calme — rappel des sorties de la semaine du lundi … », même gabarit,
    qui dit proprement qu'aucune sortie d'ampleur n'est parue et rappelle les sorties du dernier
    bulletin (mémorisées dans `bulletin/etat.json`, clé `dernier_lot`).
  - `MODE CONGRES_EN_COURS` (congrès de **niveau 1** : SFAR, ESAIC Euroanaesthesia, ESICM LIVES) →
    mise à jour quotidienne du site avec les communications de la veille, **sans courriel** (supprimer
    `bulletin/courriel-AAAA-MM-JJ.html` avant le commit). Si ce jour est un samedi
    (`SAMEDI_DANS_CONGRES`), **pas de courriel hebdomadaire** non plus : le récapitulatif le remplace.
  - `MODE CLOTURE_HIER` → veille complète du congrès, Radar remis à jour, puis
    `bash outils/faire-bulletin.sh --congres="SFAR 2026"` : courriel « Récapitulatif des sorties du
    congrès "…" », envoyé à **07:50**.
  - `MODE RIEN` → elle termine sans rien modifier.
  - Les congrès de **niveau 2** (ASA Anesthesiology, SCCM Critical Care Congress, ISICEM, ATS)
    n'ont pas de mode propre : leurs sorties sont reprises par la veille du samedi.
- **Deux commits, pas un** (règle du 08/09/2026), dans tout mode qui publie : d'abord le site
  (`index.html`, `fiche/`, `sitemap.xml`, `outils/congres.json`, `journal/`), poussé aussitôt ; puis le
  bulletin (`bulletin/`), poussé à son tour. Si la fabrication du bulletin échoue, le site est déjà en ligne.
- **Journal de veille** (`journal/AAAA-MM-JJ.md`, règle du 08/09/2026) : chaque jour qui modifie le
  site, la routine écrit ce qu'elle a examiné, retenu et écarté, avec le motif. `node outils/moisson.mjs
  --json=F` puis `node outils/journal.mjs --squelette --moisson=F` dressent la liste des candidats ;
  la routine remplit Verdict et Motif de chaque ligne ; `node outils/journal.mjs --cloture` ajoute
  les cartes du jour, le bilan du contrôle qualité et les fichiers du bulletin, et **refuse tant qu'un
  candidat n'a pas de verdict**. Le journal part dans le premier commit. Le dépôt étant public, il ne
  contient que des titres, des liens PubMed et des décisions éditoriales. Voir `journal/README.md`.
- **Publications définitives et publications attendues** : en fin de rapport, `moisson.mjs` rapproche
  les sorties PubMed des cartes présentées en congrès qui n'ont pas encore de lien vers l'article
  (`DEFINITIF?`, avec une recherche ciblée par sigle d'essai, hors fenêtre de dates) et des lignes
  « publications attendues » du Radar (`RADAR?`). Une ligne `DEFINITIF?` se traite en **mettant la
  carte à jour** (`outils/BRIEF-REVISION.md`, chaîne qualité), jamais en créant une seconde carte ;
  une ligne `RADAR?` retire l'attente du Radar et traite l'article comme candidat.
- **Une page par article pour les moteurs de recherche** (`fiche/<ancre>/index.html`, règle du
  08/09/2026) : `node outils/pages-articles.mjs` fabrique, à partir des cartes d'`index.html`, une page
  statique par article (titre d'origine, accroche, résumé, résultat principal, « En pratique », fiche
  complète, liens ; description, canonical, Open Graph et JSON-LD pour Google) et tient le bloc
  `<!--FICHES:DEBUT-->…<!--FICHES:FIN-->` de `sitemap.xml`. Ces pages ne servent qu'à être trouvées :
  chacune renvoie vers le site principal, à la carte (`https://pausear.fr/#ancre`), et
  **le bouton « Partager » du site continue de pointer vers le site principal, pas vers la page par
  article**. L'ancre est la même que celle du script d'`index.html` et de
  `outils/bulletin.mjs` : trois endroits à ne jamais changer isolément. **La routine lance le script
  après toute modification d'`index.html`, avant son premier commit** (`fiche/` et `sitemap.xml`
  partent avec le site) ; le workflow `.github/workflows/fiches.yml` rattrape un oubli en régénérant et
  poussant lui-même. Rien ne change sur la page principale. `--verifier` dit si `fiche/` est à jour.
- **Chien de garde** (`.github/workflows/chien-de-garde.yml`, `outils/chien-de-garde.mjs`, règle du
  08/09/2026) : indépendant de la routine, il vérifie chaque matin à 08:00 UTC que ce qui devait être
  publié l'a été — le courriel du samedi ou du congrès sur `main`, un commit du jour pendant un
  congrès. Sinon, e-mail d'alerte au propriétaire (secrets Gmail du bulletin) et échec du workflow,
  notifié par GitHub. C'est ce qui aurait signalé le samedi sans courriel de Pause Cardio.
- **Contrôle mensuel des liens** (`.github/workflows/controle-liens.yml`, `outils/controle-liens.mjs`) :
  le 1er du mois, chaque lien des cartes et du Radar est sondé ; les liens morts (404, 410, domaine
  disparu) ouvrent une issue GitHub « Liens morts » avec la liste. Les refus d'éditeurs (403, 429)
  sont classés « incertains » et n'ouvrent rien. Corriger un lien mort = remplacer l'adresse dans la
  carte, sans toucher au texte.
- **Entretien du calendrier, pour que ça roule d'une année sur l'autre** : `congres.mjs` imprime des
  lignes `A_VERIFIER` — dates inconnues, dates non confirmées, ou édition suivante à chercher quand la
  dernière édition connue d'une famille est passée, et revérification générale le premier samedi de
  janvier. La routine traite chaque ligne le jour même : elle cherche les dates **sur le site officiel
  du congrès** (champ `source`), les inscrit dans `outils/congres.json` avec `"confirme": true`, et
  laisse `null` ce qu'elle ne trouve pas — jamais une date inventée. **Quand elle ne trouve rien, elle
  écrit `"prochaine_verification": "AAAA-MM-JJ"` (aujourd'hui + 14 jours) sur l'entrée** : jusqu'à
  cette date la ligne sort en `REPORTE` et n'est pas à retraiter (règle du 08/09/2026, pour ne pas
  refaire la même recherche tous les matins). Le champ se retire quand les dates sont confirmées.
- **Contexte de congrès sur les cartes** : quand une sortie est ajoutée pendant ou pour un congrès,
  la ligne `.meta` se termine par le sigle du congrès — « <b>Intensive Care Med</b> · 12 octobre 2026 ·
  Essai randomisé · **ESICM 2026** » — pour la distinguer des sorties ordinaires. Les sigles reconnus
  sont `SFAR`, `ESAIC`, `ESICM`, `ASA`, `SCCM`, `ISICEM`, `ATS` (script d'`index.html` **et**
  `outils/moisson.mjs`).
- **Horaires d'envoi** (`outils/envoyer-courriel.mjs`) : bulletin du samedi à **08:00**, récapitulatif
  de congrès à **07:50**, heure de Paris ; la campagne Brevo est programmée quand le courriel est
  poussé avant l'heure, envoyée immédiatement sinon (tests à la main).
- **Une fois `index.html` à jour, toujours lancer `bash outils/faire-bulletin.sh`** (voir la section
  suivante) : c'est ce qui fabrique le bulletin PDF de la semaine.
- **Avant** d'écrire quoi que ce soit : `node outils/moisson.mjs` (voir la section précédente).
- Après modification : commit + push sur `main` (index.html **et** le dossier `bulletin/`).
  GitHub Pages republie tout seul en 1–2 minutes. Les dates `lastmod` de `sitemap.xml` sont
  entretenues automatiquement par `outils/bulletin.mjs` à chaque bulletin produit.
- Toujours vérifier avant de pousser : HTML valide, liens qui fonctionnent, filtres et recherche opérants.

## Bulletin PDF hebdomadaire

Après chaque mise à jour, un **bulletin d'une à trois pages** récapitule uniquement ce qui vient d'être
ajouté — de quoi être lu en deux minutes ou transféré à des collègues.

- Une seule commande, à lancer depuis la racine du dépôt : `bash outils/faire-bulletin.sh`
- La même commande écrit aussi **`bulletin/courriel-AAAA-MM-JJ.html`** : le bulletin en version
  e-mail (tableaux, styles en ligne), où chaque titre renvoie vers sa fiche sur
  `https://pausear.fr/#ancre-de-l-article`. C'est **la voie principale d'envoi** : le
  workflow `.github/workflows/bulletin-inscrits.yml` l'expédie à la liste Brevo « Bulletin Pause AR »
  dès qu'il arrive sur `main`, via `outils/envoyer-courriel.mjs`. Il faut pour cela les secrets
  `BREVO_CLE_API`, `BREVO_LISTE_ID` et `BREVO_EXPEDITEUR` (Settings → Secrets → Actions) ; si l'un
  manque, l'envoi est ignoré
  sans erreur. Le pied du courriel doit garder le lien `{{ unsubscribe }}`, que Brevo remplace chez
  chaque destinataire. `--apercu` produit aussi `courriel-apercu.html`, jamais envoyé.
- **Gabarit du courriel** (refondu le 01/09/2026) : sujet et en-tête « Les sorties de la semaine du
  lundi … » (le lundi de la semaine couverte), articles **rangés par surspécialité** comme sur la
  plateforme — en-tête au nom de la surspécialité dans sa couleur, puis ses articles dans l'ordre de
  la page — **sans aucun badge de niveau** : chaque bloc commence directement par le titre (accroche,
  repère revue · date, « Résultat principal », lien « Lire la fiche » dessous ; fond teinté conservé
  pour les recommandations). Avec `--congres="…"`, le sujet devient « Récapitulatif des sorties du
  congrès "…" ». Ne pas réintroduire les badges de niveau dans le courriel.
- **Ancres des articles** : le script d'`index.html` donne à chaque carte un `id` tiré de son titre
  (minuscules sans accents, tirets, 64 caractères max, suffixe `-2` en cas de doublon) et ouvre
  tiroir + fiche à l'arrivée sur `#ancre`. La **même règle vit dans `outils/bulletin.mjs`**
  (fonction `poserAncres`) : ne jamais changer l'une sans l'autre, les liens des bulletins déjà
  envoyés en dépendent.
- Elle compare les articles de `index.html` à ceux déjà signalés (mémorisés dans `bulletin/etat.json`,
  la comparaison se fait sur le titre) et ne garde que ceux **parus dans les 7 derniers jours** (voir
  plus bas) :
  - **s'il y a du nouveau** → écrit `bulletin/bulletin-AAAA-MM-JJ.html`, l'imprime en PDF avec Chromium,
    met à jour la page d'archives `bulletin/index.html` et pose le lien « 📄 Bulletin du … » dans la ligne
    d'en-tête du tableau de bord ;
  - **s'il n'y a rien de neuf** → affiche `RIEN` et n'écrit rien… sauf avec `--rappel` (le samedi) :
    il écrit alors le courriel « Semaine calme » qui rappelle les sorties du dernier bulletin
    (`RAPPEL`), sans PDF ni entrée d'archives.
- Contenu d'une entrée du PDF : surspécialité, type d'étude, titre (cliquable vers l'article
  original), journal et date, résumé, et l'encadré **« En pratique »** repris de la fiche de lecture.
  **Aucun niveau affiché** (règle du 12/09/2026) : l'étiquette « Changement de pratique » /
  « À connaître » / « Veille » a été retirée du bulletin, comme elle l'avait été de la plateforme le
  29/08/2026 et du courriel le 01/09/2026 — le lecteur n'a pas à lire notre verdict éditorial sur
  chaque article. Les niveaux restent obligatoires sur les cartes : ils servent au **classement** des
  entrées (crit → warn → watch) et au chapeau. Ne pas réintroduire le badge.
  **Le fond légèrement teinté est réservé aux recommandations** (`article.e.reco`, `#f1f7f6`, la même
  teinte que dans le courriel), et non plus au niveau « changement de pratique » : le PDF suit ainsi la
  règle de la plateforme, où le rouge `--status-critical` ne sert qu'aux fiches.
  Ordre : crit → warn → watch. Le pied de page — du PDF comme du courriel — porte la mention
  « **Fiches rédigées à l'aide de l'IA** — résumés à valider par le lecteur avant toute application
  clinique » (jamais « rédigées par Claude », règle du 01/09/2026).
- Le PDF est publié avec le site : `https://pausear.fr/bulletin/` liste tous les
  bulletins, le plus récent en tête.
- Les fichiers de `bulletin/` (PDF, `index.html`, `etat.json`) doivent être **commités** : c'est `etat.json`
  qui évite de re-signaler la semaine suivante les articles déjà annoncés.
- **Ce qui fait foi, c'est la date de parution dans la revue** (règle du 04/09/2026), lue dans la
  ligne `.meta` de la carte : le bulletin et le courriel ne signalent que les articles **inconnus et
  parus dans la semaine écoulée**, du samedi précédent (jour du dernier courriel) au samedi de la
  routine inclus (le script imprime la ligne `FENETRE`). Tout article déjà annoncé par un courriel
  précédent est retiré, même s'il tombe dans la fenêtre. Un article ajouté après coup — rattrapage
  d'une année, nouvelle surspécialité, reprise tardive — est **mémorisé sans être annoncé** (ligne
  `HORS_SEMAINE` dans la sortie du script) : s'il n'y a que cela, la semaine reste « calme ». Une carte
  sans jour dans `.meta` est écartée de la même façon : toujours écrire le jour pour une sortie de la
  semaine. **C'est ce qui fait que les cartes de l'édition zéro ne partiront pas dans un courriel.**
- Autres commandes utiles : `node outils/bulletin.mjs --init` (remémorise tous les articles actuels sans
  produire de bulletin — à lancer une fois l'édition zéro terminée, puis seulement en cas de remise à
  zéro), `bash outils/faire-bulletin.sh --apercu` (bulletin d'essai à partir des 5 dernières sorties de
  l'année en cours, ne touche à rien d'autre), `--date=AAAA-MM-JJ` pour forcer la date du bulletin,
  `--congres="SFAR 2026"` pour le titre récapitulatif de congrès, `--rappel` pour le courriel
  « Semaine calme » quand rien n'est neuf (`--apercu --rappel` pour en voir un exemple).
- Les repères `<!--BULLETIN:DEBUT-->` / `<!--BULLETIN:FIN-->` d'`index.html` restent en place mais
  **vides** : depuis le 29/08/2026 le tableau de bord n'affiche plus de lien vers le PDF ni vers les
  archives (le bulletin part par courriel ; `/bulletin/` reste accessible par adresse directe).
  Ne pas les retirer, `outils/bulletin.mjs` s'en sert toujours.

## Être trouvé — moteurs de recherche et assistants (règle du 12/09/2026)

Le site n'existe que pour être trouvé et repris. Quatre pièces, toutes à la racine, à ne pas retirer :

- **`robots.txt`** : tout est ouvert. Les blocs nommés (Googlebot, Bingbot, mais aussi `GPTBot`,
  `OAI-SearchBot`, `ClaudeBot`, `Claude-SearchBot`, `PerplexityBot`, `Google-Extended`,
  `Applebot-Extended`, `CCBot`, `meta-externalagent`…) **ne restreignent rien** — `User-agent: *
  Allow: /` les couvrait déjà. Ils disent explicitement « oui » aux robots qui cherchent leur propre
  nom, et rendent l'intention lisible. Ne jamais y écrire de `Disallow` sans demande explicite : ce
  serait se retirer des réponses des assistants.
- **`llms.txt`** : la fiche d'identité du site pour les modèles de langue (convention llmstxt.org) —
  ce que le site contient, comment une page d'article est structurée, **comment le citer** (citer
  l'article original ; Pause AR pour la synthèse française), les limites, le rythme. À mettre à jour
  quand le nombre de surspécialités ou le rythme change.
- **`methode/index.html`** : la page « Comment Pause AR est fabriqué » — sélection, rédaction,
  double contrôle, rôle de l'IA, ce que les fiches ne sont pas, indépendance et vie privée. C'est la
  page que Google lit pour juger du sérieux d'un site médical, et celle qu'un assistant cite quand on
  lui demande ce qu'est Pause AR. **Écrite à la main, pas régénérée** : la mettre à jour quand la
  méthode change. Liée depuis le pied de page du tableau de bord et de chaque page d'article, et
  présente dans `sitemap.xml`.
- **`404.html`** : page d'erreur maison, en `noindex, follow`, qui renvoie vers le tableau de bord, la
  méthode et les bulletins.
- **Le fichier `<clé>.txt`** (32 caractères hexadécimaux) : la clé IndexNow. **Ce n'est pas un secret**,
  et sa place n'est pas dans les secrets GitHub : le protocole exige qu'elle soit servie publiquement
  par le site, c'est ainsi que le moteur vérifie qui annonce les adresses. Un seul fichier de ce nom à
  la racine, contenant exactement la clé qui lui donne son nom — `outils/indexnow.mjs` refuse de partir
  sinon.

**Prévenir les moteurs dès la publication** (règle du 13/09/2026) : `outils/indexnow.mjs` annonce les
adresses nouvelles ou modifiées à Bing, Yandex, Seznam et Naver, qui partagent le même point d'entrée
(protocole IndexNow). Sans argument, il annonce les cartes du jour (`data-ajout` = aujourd'hui) et la
page d'accueil ; `--tout` couvre le plan du site, `--essai` montre sans envoyer. Le workflow
`.github/workflows/indexnow.yml` le lance à chaque arrivée sur `main` touchant `index.html`, `fiche/`
ou `methode/`. **Google ne participe pas à IndexNow** — pour lui, c'est `sitemap.xml` et son propre
rythme qui font foi ; ce n'est donc pas un raccourci vers Google, mais c'est le chemin le plus court
vers les réponses de ChatGPT et de Copilot, qui s'appuient sur l'index de Bing. Le script et le
workflow **n'échouent jamais** : prévenir un moteur est un confort, pas une condition de publication.

Sur chaque page (`index.html`, pages d'article, méthode) :
`<meta name="robots" content="index, follow, max-snippet:-1, max-image-preview:large,
max-video-preview:-1">` — c'est ce qui autorise Google à afficher un extrait long et une grande
vignette plutôt qu'une ligne tronquée.

**Données structurées** (JSON-LD, `outils/pages-articles.mjs` pour les fiches, en dur dans
`index.html` pour l'accueil) :

- accueil : `WebSite`, `Organization` (avec `knowsAbout`, `email`, et `publishingPrinciples` qui
  pointe sur `/methode/`), `CollectionPage`, `AboutPage`, `Periodical` ;
- page d'article : `Article` + `BreadcrumbList`. L'`author` est **l'organisation, jamais une
  personne** — le site ne nomme personne. `keywords` reprend le `data-kw` bilingue de la carte, et
  `isBasedOn` **et** `citation` pointent tous deux sur le DOI ou le lien PubMed de l'article résumé :
  la fiche est une synthèse, elle ne se substitue pas à la source.

**Ne jamais déclarer une donnée structurée fausse.** En particulier, pas de `SearchAction` tant que
la recherche du site n'a pas d'adresse partageable (`?q=…`) : elle est aujourd'hui purement locale au
navigateur.

## Fréquentation du site

Compteur **GoatCounter** : une balise `<script data-goatcounter=…>` juste avant `</body>` dans
`index.html` et dans la page d'archives `bulletin/index.html` (gabarit dans `outils/bulletin.mjs`,
fonction `rendreArchives`, pour qu'elle survive à chaque régénération). Sans cookie ni bandeau de
consentement. Ne pas ajouter d'autre outil de mesure sans demande explicite.

## Envoi du bulletin par e-mail

Deux voies, toutes deux automatiques :

1. **Aux inscrits, par Brevo** — la voie principale (voir ci-dessus,
   `.github/workflows/bulletin-inscrits.yml` + `outils/envoyer-courriel.mjs`). Réglages, à enregistrer
   une seule fois dans **Settings → Secrets and variables → Actions** :
   - `BREVO_CLE_API` — la clé API Brevo (SMTP & API → Clés API → Générer) ;
   - `BREVO_LISTE_ID` — le numéro de la liste « Bulletin Pause AR » ;
   - `BREVO_EXPEDITEUR` — l'adresse d'envoi des campagnes, qui doit être **validée dans Brevo**
     (Expéditeurs & IP). Elle n'est écrite nulle part dans le dépôt : `outils/envoyer-courriel.mjs`
     la lit dans l'environnement, et **s'arrête proprement sans envoyer** si l'un des trois secrets
     manque, plutôt que de faire échouer le workflow au milieu d'un appel API.
   `node outils/etat-brevo.mjs` affiche l'état du compte et des listes.
2. **Le PDF au propriétaire, par Gmail** — `.github/workflows/bulletin-mail.yml` +
   `outils/envoyer-bulletin.py` (Python standard, SMTP Gmail), déclenché par les pushes vers `main` qui
   touchent `bulletin/bulletin-*.pdf`. Secrets : `GMAIL_ADRESSE`, `GMAIL_MOT_DE_PASSE_APPLICATION`
   (« mot de passe d'application » Gmail, 16 lettres, à créer sur myaccount.google.com/apppasswords),
   et facultativement `BULLETIN_DESTINATAIRES` (adresses séparées par des virgules ; **absent = le
   bulletin part vers `GMAIL_ADRESSE`**). Ces mêmes secrets Gmail servent au chien de garde.

**Le dépôt est public** : ni clé, ni adresse d'inscrit, ni adresse de destinataire ne doivent jamais
figurer dans un fichier ou un journal — uniquement dans les secrets GitHub, et les destinataires sont
mis en copie cachée. Si les identifiants ne sont pas (encore) enregistrés, les scripts affichent
« envoi ignoré » et se terminent normalement — pas d'échec rouge ni de notification d'erreur.
Envoi manuel de test : onglet **Actions → Bulletin aux inscrits → Run workflow**, avec l'option
`essai` à `true` pour ne rien envoyer. En local : `node outils/envoyer-courriel.mjs --essai` et
`python3 outils/envoyer-bulletin.py --essai`.

## Sources de veille

Revues (abréviations PubMed vérifiées sur le catalogue NLM, listes `MAJEURES` et `SPECIALISEES`
d'`outils/moisson.mjs`) :

- **Grandes revues, tous types d'articles** : NEJM (`N Engl J Med`), The Lancet (`Lancet`), JAMA,
  BMJ, Nature Medicine (`Nat Med`), NEJM Evidence (`NEJM Evid`), Lancet Respiratory Medicine
  (`Lancet Respir Med`), Intensive Care Medicine (`Intensive Care Med`), American Journal of
  Respiratory and Critical Care Medicine (`Am J Respir Crit Care Med`), Critical Care Medicine
  (`Crit Care Med`), Anesthesiology, British Journal of Anaesthesia (`Br J Anaesth`).
- **Revues de surspécialité, essais / recommandations / méta-analyses seulement** : Anaesthesia,
  Anesthesia & Analgesia (`Anesth Analg`), European Journal of Anaesthesiology (`Eur J Anaesthesiol`),
  Anaesthesia Critical Care & Pain Medicine (`Anaesth Crit Care Pain Med`), Annals of Intensive Care
  (`Ann Intensive Care`), Critical Care (`Crit Care`), Critical Care Explorations (`Crit Care Explor`),
  Chest, European Respiratory Journal (`Eur Respir J`), Resuscitation, Annals of Emergency Medicine
  (`Ann Emerg Med`), Regional Anesthesia and Pain Medicine (`Reg Anesth Pain Med`), Journal of Clinical
  Anesthesia (`J Clin Anesth`), Neurocritical Care (`Neurocrit Care`), Journal of Neurotrauma
  (`J Neurotrauma`), Journal of Trauma and Acute Care Surgery (`J Trauma Acute Care Surg`),
  International Journal of Obstetric Anesthesia (`Int J Obstet Anesth`), American Journal of Obstetrics
  and Gynecology (`Am J Obstet Gynecol`), Paediatric Anaesthesia (`Paediatr Anaesth`), Pediatric
  Critical Care Medicine (`Pediatr Crit Care Med`), Annals of Surgery (`Ann Surg`), JAMA Surgery
  (`JAMA Surg`), Lancet Infectious Diseases (`Lancet Infect Dis`).

Recommandations : **SFAR** (sfar.org), **ESAIC** (esaic.org), **ESICM** (esicm.org), **Surviving Sepsis
Campaign / SCCM** (sccm.org), **ASA** (asahq.org), **ERC / ILCOR** (erc.edu, ilcor.org).

Congrès (calendrier tenu dans `outils/congres.json`, deux niveaux) :

- **niveau 1**, couverture quotidienne et récapitulatif le lendemain de la clôture : **SFAR** (congrès
  national d'anesthésie et de réanimation), **ESAIC Euroanaesthesia**, **ESICM LIVES**.
- **niveau 2**, repris par la veille du samedi : **ASA Anesthesiology**, **SCCM Critical Care
  Congress** (décision du 11/09/2026 : il reste en niveau 2), **ISICEM** (Bruxelles), **ATS**.

## Historique

- 13/09/2026 — **reprise de langue de tout le stock** : les 169 cartes passées par la troisième passe
  de la chaîne qualité (`outils/BRIEF-LANGUE.md`), par lots de 12, chaque lot confié à un relecteur
  distinct qui n'avait pas la source. 164 cartes réécrites, 5 laissées telles quelles, **aucune
  refusée** par le garde-fou de `outils/revision/langue-appliquer.py` — aucun chiffre, aucune section
  de fiche, aucune puce, aucune mention finale n'a bougé. 455 corrections, et **23 doutes de fond
  signalés sans être tranchés**, consignés dans `revision/2026-09-13/journal.md` : ils repartent au
  relecteur de fidélité, plusieurs sont de vraies erreurs (borne d'intervalle de confiance inversée,
  épisodes présentés comme une durée, risque relatif incohérent avec ses propres pourcentages).
  Trois défauts d'outillage trouvés au passage, tous corrigés : `outils/revision/extraire.py` portait
  encore les surspécialités de Pause Cardio ; la règle « référence géographique » différait entre les
  deux dépôts ; et `\b` ne connaissant que l'ASCII en JavaScript, `/\b[ée]vidence\b/` ne s'était
  jamais déclenchée sur « évidence ».

- 10/09/2026 — création de Pause AR à partir du dépôt terminé de Pause Cardio : même architecture,
  mêmes outils, même chaîne qualité, même routine ; logo « pause dans la capnographie » et couleur de
  marque bleu-vert (#0d6e6b / #3aada6), nouvelles sources et nouveaux congrès.
- 11/09/2026 — dixième surspécialité (`sedation`, détachée d'`arret`), puis **édition zéro** :
  rattrapage 2025–2026, **2 189 candidats examinés, 162 retenus** (périop 18, ventilation 20,
  sepsis 19, ALR 17, hémodynamique 16, neuro-traumatologie 15, arrêt 15, pédiatrie 15, sédation 14,
  obstétrique 13). Journal dans `journal/2026-09-11.md`. Chaque carte rédigée à partir du résumé
  PubMed, relue par un agent distinct, puis `controle-cartes.mjs --tout --sources` : 0 erreur.
  Une fois l'édition zéro publiée, `node outils/bulletin.mjs --init` a mémorisé les 162 articles
  pour qu'aucun ne parte dans un courriel hebdomadaire.
- 12/09/2026 — **inscription branchée** : formulaire Brevo « Bulletin Pause AR » (double opt-in) posé
  dans `ABO_FORM`, l'encart d'inscription s'affiche donc enfin. Adresse d'expédition
  `contact@pausear.fr` créée chez OVH. Le compte transactionnel de Brevo doit être activé par leur
  support avant que la double confirmation soit disponible : sur un compte neuf, les deux options sont
  grisées et Brevo coche « pas d'e-mail de confirmation », qu'il ne faut pas garder.
  **Chaîne d'envoi vérifiée de bout en bout le 12/09/2026** : les trois secrets enregistrés, essai à
  blanc concluant, puis envoi réel du bulletin du 12 septembre à la liste (campagne Brevo id 5,
  « ENVOYÉ »). C'est précisément ce qui n'avait jamais été vérifié sur Pause Cardio, où la routine
  « réussissait » chaque matin sans que rien ne parte.
- 11/09/2026 — **nom de domaine `pausear.fr`** (OVH) branché sur GitHub Pages : quatre enregistrements
  A vers les machines de GitHub, `www` en CNAME, enregistrements IPv6 d'OVH retirés, messagerie OVH
  (MX, SPF, DKIM, SRV) laissée intacte. Le site passe de `hacuubo.github.io/PAUSE-AR/` à la racine de
  `pausear.fr` : toutes les adresses du dépôt réécrites, fichier `CNAME` ajouté.
- 11/09/2026 — **trois défauts corrigés dans `outils/controle-cartes.mjs`**, hérités de Pause Cardio
  et révélés par le volume : `--sources` interrogeait PubMed sans frein (au-delà de trois appels par
  seconde PubMed coupe, et l'appel échouait en silence — sur 162 cartes, 148 recherches ratées et
  aucune fidélité réellement contrôlée) ; la comparaison de titre sur 40 caractères acceptait un
  homonyme, au point de juger une carte sur les chiffres d'un autre article ; et les nombres écrits
  en toutes lettres dans les résumés anglais (« Eighty-seven randomised trials ») étaient invisibles,
  ce qui refusait des chiffres pourtant exacts. **Ne pas retirer le frein de 350 ms.**
