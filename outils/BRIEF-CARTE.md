# Brief — rédaction d'une carte Pause AR

**Lire d'abord `outils/CHARTE-REDACTION.md`** : elle fixe le ton, la langue et les règles de fidélité
scientifique de tout texte français ; ce brief ne décrit que le format et la livraison.

Tu rédiges des cartes HTML pour un tableau de bord de veille bibliographique en anesthésie-réanimation
(lecteurs : anesthésistes-réanimateurs et médecins francophones). Une carte = un article. Tu reçois pour chaque article son
résumé PubMed officiel dans `abs/<PMID>.txt` (titre, revue, date, DOI, abstract).

## Règles absolues

1. **Aucun chiffre inventé.** Chaque HR, IC 95 %, pourcentage, effectif vient de l'abstract fourni
   (ou de l'article original que tu peux consulter via le DOI avec WebFetch). Si un chiffre n'est pas
   dans tes sources, tu ne l'écris pas. Mieux vaut une fiche courte qu'un chiffre faux.
2. **Titre de l'article en anglais, tel quel** (copie exacte du champ TITRE). Tout le reste en français.
3. **Aucune référence géographique ou personnelle** (pas de « notre centre », « en France » comme
   contexte de pratique…). Aucune mention d'auteur ni d'IA dans la carte.
4. Typographie française : espace insécable (`&nbsp;`) avant `:` `;` `?` `!` `%`, guillemets « »,
   HR/IC écrits « HR 0,72 ; IC95 % 0,60–0,86 ». Décimales avec virgule. Écrire les entités HTML pour
   les accents dans les blocs `.fiche` ET ailleurs (`&eacute;`, `&agrave;`, `&rsquo;`…) — ou en UTF-8
   direct, les deux sont acceptés, mais pas de caractère cassé.
5. Le lien « Article original » = `https://doi.org/<DOI>`. Pas d'autre lien sauf si tu l'as
   réellement ouvert et vérifié.

## Format exact (copier la structure, remplacer le contenu)

```html
<article class="card" data-ajout="AAAA-MM-JJ (date du jour)" data-spec="<spec donné dans ta liste>" data-year="AAAA" data-lvl="crit|warn|watch" data-kw="mots-clés bilingues FR EN avec acronymes" data-fr="Accroche française de 6 à 10 mots, verbe conjugué, lisible à voix haute">
      <div class="top">
        <span class="badge warn">&#9650; &Agrave; conna&icirc;tre</span>
        <span class="type">Essai randomis&eacute; &middot; ACRONYME</span>
      </div>
      <h3>Titre anglais exact</h3>
      <div class="meta"><b>Revue</b> &middot; 10 avril 2025 &middot; complément court (effectif, pays, design)</div>
      <p class="sum">Résumé français de 2 à 4 phrases : population, intervention, résultat principal chiffré, portée.</p>
      <div class="cle">Le chiffre clé, repris mot pour mot de la fiche, nombres en <b>gras</b> — ex. : Mortalité à 90&nbsp;jours&nbsp;: <b>31,4&nbsp;%</b> sous stratégie restrictive contre <b>32,8&nbsp;%</b> (différence non significative) &middot; jours vivant sans ventilation à J28&nbsp;: <b>18,1</b> contre <b>16,4</b></div>
      <div class="actions">
        <button class="btn primary" onclick="toggle(this)">&#128196; Fiche de lecture</button>
        <a class="btn" href="https://doi.org/DOI" target="_blank" rel="noopener">Article original &#8599;</a>
      </div>
      <div class="fiche">
        <h4>Question clinique</h4>
        <p>…</p>
        <h4>M&eacute;thode</h4>
        <p>Design, population, effectif, comparateur, critère principal, durée de suivi.</p>
        <h4>R&eacute;sultats cl&eacute;s</h4>
        <ul>
          <li>Critère principal chiffré (HR, IC95 %, valeurs absolues, NNT si calculable à partir des chiffres fournis).</li>
          <li>Critères secondaires et sécurité.</li>
        </ul>
        <h4>Limites</h4>
        <ul>
          <li>…</li>
        </ul>
        <div class="verdict"><b>En pratique&nbsp;:</b> ce que ça change concrètement pour un anesthésiste-réanimateur au quotidien, 2 à 3 phrases.</div>
        <div class="sig">R&eacute;sum&eacute; &agrave; valider par le lecteur avant application clinique — se reporter &agrave; l&rsquo;article original en lien.</div>
      </div>
    </article>
```

Détails de format :
- Badges selon le niveau : `crit` → `<span class="une">&#9733; &Agrave; la une</span> <span class="badge crit">&#9679; Changement de pratique</span>` ;
  `warn` → `<span class="badge warn">&#9650; &Agrave; conna&icirc;tre</span>` ;
  `watch` → `<span class="badge watch">&#9678; Veille</span>`.
- `span.type` : « Essai randomis&eacute; », « Recommandations » ou « Consensus d'experts » (pour un
  statement/position paper, écrire « Recommandations » suivi de la société : « Recommandations SFAR »),
  « M&eacute;ta-analyse ». Ajouter « &middot; ACRONYME » si l'essai en a un.
- `.meta` : **jour + mois en toutes lettres + année** de la parution en ligne (champ ArticleDate ;
  sinon date PubMed). Le jour est obligatoire.
- `data-year` : l'année de parution.
- Pour une recommandation / un consensus sans chiffre unique : **omettre** le bloc `<div class="cle">`.
- `data-kw` : 15 à 30 mots-clés **bilingues FR + EN**, avec les acronymes de la spécialité dans les deux
  langues (SDRA/ARDS, PEP/PEEP, ALR/regional anesthesia, RCP/CPR, TC/TBI, AG/general anesthesia, ECMO,
  VNI/NIV, IRA/AKI…) et les noms de molécules.

## Contexte de congrès (règle du site)

**Si et seulement si** le résumé PubMed ou la page de l'article (DOI, ouverte avec WebFetch) indique que
l'essai a été présenté à l'un des congrès suivis (SFAR, ESAIC, ESICM, ASA, SCCM, ISICEM, ATS), la ligne
`.meta` se termine par le sigle du congrès et son millésime — ex. : `<b>NEJM</b> &middot; 20 octobre 2026
&middot; Essai randomis&eacute; &middot; ESICM 2026`.
Sinon, ne rien ajouter. Ne jamais l'écrire par supposition.

## Niveau à respecter

Le niveau de chaque carte t'est donné dans ta liste (provisoire s'il a été fixé sans lire l'abstract). Rappel du
sens : `crit` = susceptible de changer la pratique (essai pivot positif, ou clairement négatif sur une
pratique courante) ; `warn` = à connaître ; `watch` = veille (phase 2, analyse exploratoire, observationnel).
Si l'abstract te fait penser que le niveau est faux, applique le niveau que tu juges juste et dis-le
dans ton rapport avec la raison. Dans le doute entre deux niveaux, prends le plus haut.

## Livraison

Écris chaque carte dans un fichier `out/<PMID>.html` (dossier `out/` à côté de `abs/`) (uniquement le bloc `<article>…</article>`).
Termine par un court rapport : pour chaque carte, la source de chaque chiffre (abstract / article
consulté), et tout ce que tu n'as pas pu vérifier (et as donc omis).

## Année et date

`data-year` doit être **2025 ou 2026** (les seules années affichées). Quand la date de mise en ligne
(ArticleDate) est en 2024 alors que l'article paraît dans un numéro 2025, utiliser la **date PubMed**
(numéro) dans `.meta` et `data-year="2025"`. Ne jamais mettre 2024.
