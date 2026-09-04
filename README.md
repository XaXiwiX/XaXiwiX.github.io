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

## Les deux adresses e-mail doivent exister avant la vérification

Elles ne sont pas encore créées chez Combell. Une adresse citée sur le site et
dans la fiche App Store qui rebondit est un mauvais signal de vérification.

- `contact@xiwix.be` — une simple redirection suffit. Elle est publique, donc
  exposée aux robots ; en cas d'abus, une redirection se supprime et se recrée.
- `support@xiwix.be` — citée trois fois dans la politique de confidentialité et
  dans la fiche App Store. Prévoir un envoi **depuis** cette adresse (boîte
  réelle, ou « send as » dans le client de messagerie) : avec une simple
  redirection, les réponses partent de la boîte personnelle et la dévoilent à
  chaque utilisateur qui écrit.

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

## Publier

Le dépôt est `XaXiwiX.github.io` — un *user site*, servi à la **racine** du
domaine. Un dépôt de projet aurait servi sous `/xiwix.be/`, et les chemins absolus
des pages (`/assets/xiwix.css`) auraient cassé tant que le domaine n'est pas
branché. Public, parce que Pages sur dépôt privé exige un plan payant.

1. Pousser sur `main`, puis Settings → Pages → source `main` / `/`.
2. Vérifier le site sur `https://xaxiwix.github.io/` — **avant** de toucher au DNS.
3. Settings → Pages → Custom domain : `xiwix.be`. GitHub écrit alors le fichier
   `CNAME` et signale que le DNS ne pointe pas encore vers lui : c'est normal.
4. **Ensuite seulement**, basculer le DNS chez Combell.
5. Propagation faite, cocher **Enforce HTTPS**. Le certificat Let's Encrypt est
   émis automatiquement, sous 15 minutes à 24 heures.

L'ordre compte deux fois. Basculer le DNS avant l'étape 3 ferait répondre une 404
de GitHub sur `xiwix.be` pendant toute la fenêtre. Et committer un fichier `CNAME`
avant l'étape 2 ferait rediriger `xaxiwix.github.io` vers un domaine qui sert
encore la page de parking — plus de prévisualisation possible.

## DNS chez Combell

Les serveurs de noms restent ceux de Combell. Le nom d'hôte se saisit **sans le
domaine** (`www`, jamais `www.xiwix.be`).

À remplacer — l'unique `A` de l'apex (`217.21.190.139`) devient quatre `A`,
nom d'hôte vide :

    185.199.108.153
    185.199.109.153
    185.199.110.153
    185.199.111.153

À remplacer — `www` : supprimer son `A`, puis créer un `CNAME` vers
`xaxiwix.github.io.` (un même nom ne peut pas porter un `A` et un `CNAME`).

**À ne toucher sous aucun prétexte** : les deux `MX` (`mx.mailprotect.be`,
`mx.backup.mailprotect.be`), les quatre `SRV`, et les `CNAME` `autoconfig`,
`autodiscover` et `mail`. La boîte professionnelle en dépend et il n'y en a
qu'une. Laisser `ftp` en place.

Aucun enregistrement `CAA` n'existe sur le domaine : rien ne bloquera l'émission
du certificat.

## Prévisualiser en local

    python3 -m http.server 4321 --directory ~/xiwix.be

(déclaré aussi dans `~/.claude/launch.json` sous le nom `xiwix-site`)
