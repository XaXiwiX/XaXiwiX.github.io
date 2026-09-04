# xiwix.be — site de XiwiX SRL

Trois pages statiques, sans dépendance externe ni script, servies par GitHub Pages.

```
index.html              accueil — la société. NE MENTIONNE PAS 421.
421/support/            support de l'application (URL de support App Store)
421/confidentialite/    politique de confidentialité — PAS ENCORE ÉCRITE, voir plus bas
404.html                page d'erreur
assets/xiwix.css        feuille unique
CNAME                   xiwix.be
```

## Règle qui ne se négocie pas

**L'accueil ne parle jamais du jeu.** XiwiX a des clients dans un autre métier.
La vérification Apple porte sur la légitimité de la société, pas sur le fait que
le site annonce l'application. Les pages `/421/` existent, ne sont liées depuis
nulle part, et se rejoignent par leur URL directe — c'est tout ce qu'Apple demande.

Contrôle avant chaque publication :

```sh
grep -i "421\|jeu\|application\|App Store" index.html   # doit ne rien renvoyer
```

## Ce qui reste à compléter

Les valeurs manquantes sont surlignées en jaune dans la page (`<span class="todo">`),
pour qu'aucune ne parte en production sans être vue. Contrôle :

```sh
grep -rn "class=\"todo\"" --include="*.html" .          # doit ne rien renvoyer avant publication
```

- adresse du siège social (accueil + pied de page)
- numéro de téléphone public
- numéro d'entreprise BCE
- délai de réponse annoncé sur la page de support

Ces coordonnées doivent être **identiques** à celles déclarées dans le statut
« trader » du DSA chez Apple, qui les affiche sur la fiche publique du jeu.

## La politique de confidentialité n'est pas encore ici, exprès

La source est `~/le-bon/CONFIDENTIALITE.md` (FR/NL/EN). Elle ne peut pas être
publiée telle quelle : `~/le-bon/PUBLIER-SUR-IOS.md` relève trois défauts de fond,
dont deux ne sont pas des défauts de rédaction mais des écarts entre le texte et
le comportement du logiciel.

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

1. `gh repo create XaXiwiX/xiwix.be --public --source=. --push`
   (Pages sur dépôt privé exige un plan payant, que le compte n'a pas.)
2. Dépôt → Settings → Pages → Source : `Deploy from a branch`, branche `main`, dossier `/`.
3. Settings → Pages → Custom domain : `xiwix.be`. GitHub signalera que le DNS ne
   pointe pas encore vers lui — c'est normal à ce stade.
4. **Ensuite seulement**, basculer le DNS chez Combell (voir ci-dessous).
5. Une fois la propagation faite, cocher **Enforce HTTPS**. Le certificat
   Let's Encrypt est émis automatiquement, sous 15 minutes à 24 heures.

L'ordre compte : basculer le DNS avant l'étape 3 ferait répondre une erreur 404
de GitHub sur `xiwix.be` pendant toute la fenêtre.

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
