# ASCON-AEAD128 — Modélisation Matérielle en SystemVerilog

Ce dépôt présente une implémentation matérielle hiérarchisée de l'algorithme de chiffrement léger **ASCON-AEAD128**, standardisé par le NIST pour les environnements contraints.

Le projet repose sur une **approche modulaire en SystemVerilog**, accompagnée d'une validation fonctionnelle par simulation (ModelSim), dans le cadre d’un enseignement de **conception numérique**.

---

## 🔐 Présentation de l'algorithme ASCON

ASCON est un algorithme de **chiffrement authentifié avec données associées (AEAD)**, structuré autour d’un **état interne de 320 bits** représenté par 5 registres de 64 bits. Il opère selon un schéma de permutation successives permettant de garantir simultanément :

- la **confidentialité** via un chiffrement par blocs,
- l’**intégrité** via un tag d’authentification.

Formellement, l’état est noté :
\[
S = \{S_0, S_1, S_2, S_3, S_4\} \in \mathbb{F}_2^{320}
\]

Les quatre phases principales sont :
1. **Initialisation** par injection de la clé, du nonce et d’un vecteur IV,
2. **Absorption des données associées** (optionnelles),
3. **Chiffrement du texte clair** via des blocs successifs,
4. **Finalisation** et extraction du tag d’authenticité.

Chaque phase s’appuie sur des permutations internes notées \( p^a \), \( p^b \), combinant :
- **Ajout de constantes (pc)** : injection de nonces ou clés via XOR,
- **Substitution non linéaire (ps)** : application d'une S-box sur chaque colonne,
- **Diffusion linéaire (pl)** : propagation par rotations et XOR croisés.

---

## 🧩 Architecture matérielle

L’implémentation est organisée en modules **combinatoires et séquentiels** :

- `xor_begin.sv`, `xor_end.sv` : injection conditionnelle des données et clés,
- `pc.sv`, `ps.sv`, `pl.sv` : transformations cryptographiques internes,
- `transformation_inter.sv`, `transformation_finale.sv` : enchaînement structuré des blocs,
- `fsm_moore.sv` : pilotage par une machine d’états synchronisée,
- `ascon_top.sv` : encapsulation complète de l’architecture.

<p align="center">
  <img src="chiffrement.jpg" alt="Structure algorithmique" width="650"/>
  <br><em>Figure — Structure fonctionnelle du chiffrement ASCON-AEAD128</em>
</p>

<p align="center">
  <img src="transformation_finale.jpg" alt="Architecture matérielle finale" width="600"/>
  <br><em>Figure — Architecture matérielle de la permutation finale</em>
</p>

---

## 🧪 Objectifs pédagogiques et techniques

- Traduire une spécification cryptographique en **structure matérielle hiérarchique**,
- Appliquer des **opérations booléennes dans \( \mathbb{F}_2^n \)** avec rigueur formelle,
- Implémenter des **transformations non linéaires et diffusions linéaires** optimisées,
- Modéliser une **machine de contrôle synchrone** pilotant un pipeline cryptographique.

---

## 📝 Rapport complet

📄 [Consulter le rapport technique (PDF)](./Rapport_ASCON_AHNANI_ALI%20(2).pdf)

Ce document détaille la modélisation mathématique, les choix d’architecture, les principes de vérification ainsi que les perspectives d’optimisation.

---

## 👤 Auteur

**Ali AHNANI**  
ISMIN – Mines Saint-Étienne (1A)  
Projet de conception numérique — Avril 2025
