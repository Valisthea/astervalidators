# ═══════════════════════════════════════════════════════
# ASTER VALIDATORS — Guide de déploiement pas à pas
# ═══════════════════════════════════════════════════════


## ÉTAPE 1 — Acheter le domaine (5 min)

### Recommandé : Namecheap (paiement crypto possible)
1. Va sur https://www.namecheap.com
2. Cherche `astervalidators.com`
3. Achète (≈ $10/an)
4. **IMPORTANT pour l'anonymat** : Active "WhoisGuard" (gratuit chez Namecheap)
   → Ça masque ton nom/email dans le WHOIS public
   → Personne ne peut remonter à toi via le domaine

### Alternative : Cloudflare Registrar (prix coûtant, pas de marge)
1. https://dash.cloudflare.com → Registrar → Register domain
2. WHOIS privacy inclus par défaut


## ÉTAPE 2 — Créer un compte Netlify anonyme (3 min)

1. Va sur https://app.netlify.com/signup
2. **NE PAS** utiliser "Sign up with GitHub" (ça lie ton GitHub perso)
3. Clique "Sign up with email"
4. Utilise une adresse email séparée (voir section EMAIL ci-dessous)
5. Choisis un nom de team neutre : "aster-validators" par exemple


## ÉTAPE 3 — Déployer le site (2 min)

1. Connecte-toi sur https://app.netlify.com
2. Tu vas voir "Want to deploy a new site without connecting to Git?"
   → Clique "Deploy manually"
   → Ou va dans : Sites → "Add new site" → "Deploy manually"
3. **Drag & drop le fichier ZIP** directement dans la zone
4. Attends 10-20 secondes → Le site est live !
5. Tu as une URL temporaire type : `random-name-123.netlify.app`
6. Vérifie que tout marche en cliquant dessus


## ÉTAPE 4 — Connecter le domaine (5 min)

### Option A : DNS via Netlify (le plus simple)

1. Dans Netlify → Site settings → Domain management
2. Clique "Add custom domain"
3. Tape `astervalidators.com` → Verify → Add domain
4. Netlify va te proposer "Set up Netlify DNS"
5. Clique dessus → il te donne 4 nameservers type :
   ```
   dns1.p06.nsone.net
   dns2.p06.nsone.net
   dns3.p06.nsone.net
   dns4.p06.nsone.net
   ```
6. Va sur Namecheap (ou ton registrar) :
   - Domain List → ton domaine → "Manage"
   - Nameservers → "Custom DNS"
   - Colle les 4 nameservers de Netlify
   - Save
7. Attends 5-30 min (max 24h) pour la propagation DNS
8. Retourne sur Netlify → le domaine passe en vert ✅
9. HTTPS s'active automatiquement (Let's Encrypt)

### Option B : DNS via Cloudflare (plus de contrôle)

1. Crée un compte Cloudflare : https://dash.cloudflare.com
2. "Add a site" → `astervalidators.com`
3. Plan Free → Continue
4. Cloudflare te donne 2 nameservers → mets-les chez Namecheap
5. Dans Cloudflare DNS, ajoute :
   ```
   Type: CNAME
   Name: @
   Target: ton-site.netlify.app
   Proxy: ON (orange cloud)
   
   Type: CNAME
   Name: www
   Target: ton-site.netlify.app
   Proxy: ON
   ```
6. Activer "Full (strict)" dans SSL/TLS
7. Activer "Always Use HTTPS"
8. Activer "Auto Minify" (HTML, CSS, JS)


## ÉTAPE 5 — Vérifier le déploiement (2 min)

Après propagation DNS :

1. ✅ https://astervalidators.com → popup disclaimer + dashboard
2. ✅ https://www.astervalidators.com → redirige vers version sans www
3. ✅ Cadenas HTTPS visible
4. ✅ Teste le partage social : https://www.opengraph.xyz/
   → Colle ton URL → tu dois voir le og-image.png
5. ✅ Test mobile : ouvre sur ton tel


## ÉTAPE 6 — Google Search Console (5 min)

1. Va sur https://search.google.com/search-console
2. "Add property" → "Domain" → `astervalidators.com`
3. Vérifie via DNS (ajoute le record TXT chez Namecheap/Cloudflare)
4. Une fois vérifié → Sitemaps → Ajoute : `https://astervalidators.com/sitemap.xml`
5. Google commencera à indexer sous 24-72h


═══════════════════════════════════════════════════════
QUESTION EMAIL — RESTER ANONYME SANS REPAYER
═══════════════════════════════════════════════════════

## Verdict : NE PAS utiliser @kairos-lab.org

Si quelqu'un voit un email @kairos-lab.org dans le WHOIS, les headers,
ou n'importe quel leak → le lien est fait. On veut éviter ça.

## Solution gratuite : Créer un email dédié (0€)

### Option 1 — ProtonMail (recommandé pour crypto)
- https://proton.me/mail → Plan gratuit
- Crée : `contact@protonmail.com` style `astervalidators@proton.me`
- Avantages : chiffré, suisse, respecté dans le milieu crypto
- Utilise cet email pour : Netlify, Namecheap, Search Console

### Option 2 — Tutanota
- https://tuta.com → Plan gratuit
- Même principe, basé en Allemagne, chiffré

### Option 3 — Email forwarding gratuit sur le domaine
Si tu veux un `contact@astervalidators.com` SANS payer Google Workspace :

**Avec Cloudflare Email Routing (GRATUIT) :**
1. Dashboard Cloudflare → Email → Email Routing
2. "Create address" :
   - Custom address : `contact`
   - Forward to : `ton-protonmail@proton.me`
3. Ça crée `contact@astervalidators.com` qui forward vers ProtonMail
4. Pour RÉPONDRE depuis @astervalidators.com :
   - ProtonMail gratuit ne le permet pas nativement
   - Mais tu peux juste répondre depuis ProtonMail, les gens s'en fichent

**Avec ImprovMX (GRATUIT) :**
1. https://improvmx.com → Ajoute ton domaine
2. Forward `*@astervalidators.com` → ton ProtonMail
3. Ajoute les records MX chez ton registrar/Cloudflare
4. Gratuit pour le forwarding

## Setup recommandé final :

```
1. Crée astervalidators@proton.me (GRATUIT)
   → Utilise pour Netlify, Namecheap, etc.

2. Configure Cloudflare Email Routing (GRATUIT)
   → contact@astervalidators.com → forward → astervalidators@proton.me

3. Résultat : email pro @astervalidators.com
   → 0€/mois
   → Aucun lien avec Kairos Lab
   → Zéro trace
```

═══════════════════════════════════════════════════════
CHECKLIST ANONYMAT
═══════════════════════════════════════════════════════

- [x] WhoisGuard activé sur le domaine (masque identité)
- [x] Pas de @kairos-lab.org nulle part
- [x] Compte Netlify séparé (email ProtonMail)
- [x] Pas de GitHub perso lié (deploy par ZIP, pas par repo)
- [x] Aucune mention de Kairos Lab dans le code source ✅ (vérifié)
- [x] OG image neutre (pas de branding Kairos)
- [ ] Si tu ajoutes Plausible Analytics → compte séparé
- [ ] Si tu crées un Twitter → nouveau compte dédié

═══════════════════════════════════════════════════════
RÉSUMÉ COÛTS
═══════════════════════════════════════════════════════

| Service                | Coût         |
|------------------------|-------------|
| Domaine .com           | ~$10/an     |
| Netlify hosting        | $0 (free)   |
| Cloudflare DNS         | $0 (free)   |
| ProtonMail             | $0 (free)   |
| Cloudflare Email Route | $0 (free)   |
| SSL/HTTPS              | $0 (auto)   |
| Google Search Console  | $0 (free)   |
| **TOTAL**              | **~$10/an** |
