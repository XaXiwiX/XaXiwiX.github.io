# xiwix.be — site de XiwiX SRL

Trois pages statiques, sans dépendance externe ni script, servies par GitHub Pages.

```
index.html              accueil — la société. NE MENTIONNE PAS 421.
421/support/            support de l'application (URL de support App Store)
421/confidentialite/    politique de confidentialité — PAS ENCORE ÉCRITE, voir plus bas
404.html                page d'erreur
assets/xiwix.css        feuille unique
```

## Règle qui ne se négocie pas

**L'accueil ne parle jamais du jeu.** XiwiX a des clients dans un autre métier.
La vérification Apple porte sur la légitimité de la société, pas sur le fait que
le site annonce l'application. Les pages `/421/` existent, ne sont liées depuis
nulle part, et se rejoignent par leur URL directe — c'est tout ce qu'Apple demande.

Contrôle avant chaque publication :

```sh
grep -i "421\|jeu\|application\|App Store" index.html   # doit ne rien renvoyer
grep -rn 'class="todo"' --include="*.html" .            # doit ne rien renvoyer
```

Le second attrape les valeurs laissées en attente : elles sont surlignées en
jaune dans la page (`<span class="todo">`) plutôt que devinées, pour qu'aucune
ne parte en production sans être vue.

## Identité publiée

Recoupée avec la Banque-Carrefour des Entreprises le 4 septembre 2026 :
**XIWIX**, SRL, active, Xavier Lallemand administrateur depuis le 31 mars 2023,
NACE 62.900.

| | |
|---|---|
| Siège | Rue Jules Borbouse 42, 5170 Profondeville |
| TVA / BCE | BE 0698.895.985 |
| Contact site | `contact@xiwix.be` |
| Support application | `support@xiwix.be` |
| Téléphone | **non publié sur le site — voir ci-dessous** |

Le registre écrit la commune **Profondeville**, pas Bois-de-Villers (village de
cette commune, même code postal). Le site suit le registre : c'est cette
écriture-là qu'Apple recoupe.

⚠️ **Le téléphone reste dû à Apple.** Il ne figure pas sur le site, ce qui est
libre — mais la déclaration « trader » du DSA l'exige (article 30 : nom, adresse,
**téléphone** et e-mail), et Apple l'affichera sur la fiche publique du jeu. Ce
n'est pas contournable en gardant la voie Organisation.

Les coordonnées déclarées chez Apple doivent être **identiques** à celles-ci.

## Les trois adresses e-mail

Créées le 5 septembre 2026 chez Combell, **toutes en redirection** vers
`x.lallemand@xiwix.be`. Une redirection reçoit mais ne peut pas envoyer : les
réponses partent de la boîte de destination.

| Adresse | Rôle | Publiée |
|---|---|---|
| `contact@xiwix.be` | contact société, site + déclaration « trader » | oui |
| `support@xiwix.be` | support App Store + politique de confidentialité | oui |
| `dev@xiwix.be` | compte Apple Developer — **identifiant de connexion** | **jamais** |

> **Combell publie en différé.** Une redirection créée dans le panneau n'est pas
> encore appliquée sur la plateforme `mailprotect` : les envois de test faits dans
> la minute rebondissent en `550 5.1.1 Recipient address rejected`. Comptez une à
> deux minutes avant de conclure quoi que ce soit. La zone DNS se comporte pareil.

### À faire avant la mise en vente : convertir `support@` en vraie boîte

C'est la seule des trois qui doit pouvoir **envoyer**. Tant qu'elle est une
redirection, chaque réponse à un joueur ou à une demande RGPD part de
`x.lallemand@xiwix.be` — une adresse nominative devant chaque utilisateur.

Rien ne presse : Apple ne teste jamais l'envoi, seulement que l'adresse reçoive.
La gêne commence au premier joueur qui écrit, donc après le lancement. La
conversion est indolore — supprimer la redirection, créer la boîte du même nom —
et **ne touche à rien de publié** : la chaîne `support@xiwix.be` ne change pas,
ni sur le site, ni dans la fiche App Store, ni dans la politique de
confidentialité.

`dev@` reste une redirection sans problème : elle n'a qu'à recevoir. Ne jamais
l'écrire sur le site — c'est l'identifiant du compte qui détient le programme
développeur, les certificats de signature et la fiche.

## La politique de confidentialité n'est pas encore ici, exprès

La source est `~/le-bon/CONFIDENTIALITE.md` (FR/NL/EN). Elle ne peut pas être
publiée telle quelle : `~/le-bon/PUBLIER-SUR-IOS.md` relève trois défauts, dont
deux ne sont pas des défauts de rédaction mais des écarts entre le texte et le
comportement du logiciel.

1. le décompte quotidien est déclaré « anonyme » alors qu'il ne l'est pas ;
2. *EFFACER MES DONNÉES* laisse survivre les métriques, alors que le texte
   promet un effacement complet ;
3. bases légales et destinataires manquants (celui-ci est purement rédactionnel).

Les deux premiers se corrigent soit dans le code, soit dans le texte — c'est une
décision produit qui appartient à la session 421. **Une déclaration fausse vaut
retrait de l'application, pas seulement rejet** : tant que ce n'est pas tranché,
l'URL reste libre plutôt que fausse.

Quand la page sera écrite, décommenter le lien vers elle dans `421/support/index.html`.

## En ligne depuis le 5 septembre 2026

`https://xiwix.be` — dépôt public `XaXiwiX/XaXiwiX.github.io`, servi à la racine
(un *user site*). Un dépôt de projet aurait servi sous `/xiwix.be/` et cassé les
chemins absolus des pages ; public parce que Pages sur dépôt privé exige un plan
payant que le compte n'a pas.

Certificat Let's Encrypt `CN=xiwix.be`, avec `www.xiwix.be` en nom alternatif.
*Enforce HTTPS* actif : `http://` et `www` redirigent en 301 vers `https://xiwix.be/`.

### Zone DNS, telle qu'elle est

Serveurs de noms restés chez Combell. `A` sur l'apex vers les quatre adresses de
GitHub Pages, `CNAME` `www` vers `xaxiwix.github.io`, `ftp` inchangé sur
`217.21.190.139`. **Les deux `MX`, les quatre `SRV` et les trois `CNAME` de
messagerie n'ont pas été touchés** — vérifié après l'opération, la boîte
professionnelle n'a pas bougé.

    185.199.108.153   185.199.109.153   185.199.110.153   185.199.111.153

Aucun `CAA` sur le domaine, rien ne bloquait l'émission.

### La leçon, si le domaine doit être redéclaré un jour

Le domaine a été déclaré chez GitHub **avant** que le DNS pointe vers lui. La
vérification initiale a donc échoué, et GitHub ne l'a jamais relancée seule : le
certificat n'était pas *en attente*, il n'était même pas **demandé**. Le remède
est de retirer le domaine et de le redéclarer, ce qui force une vérification
immédiate — le certificat a été émis 40 secondes plus tard.

Dans l'autre sens : ne pas committer de fichier `CNAME` avant que Pages serve le
site, sinon `xaxiwix.github.io` redirige vers un domaine qui n'est pas encore
branché et il n'y a plus de prévisualisation.

## Prévisualiser en local

    python3 -m http.server 4321 --directory ~/xiwix.be

(déclaré aussi dans `~/.claude/launch.json` sous le nom `xiwix-site`)
