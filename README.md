# Classification de textes médicaux avec NLP classique et embeddings Transformers

## Présentation générale

Ce notebook explore différentes approches pour la classification multi-classes de courts textes médicaux en français (titre + résumé).  
L’objectif est de comparer des méthodes classiques de traitement automatique du langage (Bag of Words / TF-IDF) avec des représentations modernes basées sur des modèles Transformers, et d’analyser leurs forces et limites sur une tâche de classification sémantique difficile comportant une vingtaine de classes.

Le notebook suit une démarche expérimentale progressive et contrôlée :
- Représentations NLP classiques (TF-IDF / Bag of Words)
- Embeddings génériques CamemBERT (pooling CLS et mean)
- Classifieurs linéaires et non linéaires
- Visualisation qualitative des espaces d’embedding avec UMAP

---

## Structure du notebook

Le notebook est organisé en plusieurs sections principales :

1. **Imports et configuration**  
   - Import des bibliothèques  
   - Fixation des graines aléatoires  
   - Configuration CPU / GPU

2. **Chargement et préparation des données**  
   - Chargement du jeu de données annoté  
   - Séparation entraînement / test  
   - Prétraitement des champs titre + résumé

3. **Modèles de base (Bag of Words / TF-IDF)**  
   - Vectorisation TF-IDF  
   - Classifieur Logistic Regression  
   - Évaluation en F1 macro  
   - Matrice de confusion

4. **Embeddings CamemBERT (Transformer générique)**  
   - Pooling CLS  
   - Pooling par moyenne des jetons (mean pooling)  
   - Classifieur Logistic Regression  
   - Comparaison des performances  
   - Matrices de confusion

5. **Discussion et limites**  
   - Représentation vs capacité du classifieur  
   - Chevauchement sémantique entre classes  
   - Limites des embeddings génériques  
   - Caractère stochastique d’UMAP

---

## Résultats principaux

- Les représentations Bag of Words et TF-IDF fournissent de bonnes bases de comparaison, mais échouent à capturer la structure sémantique profonde des textes.
- Les embeddings CamemBERT génériques, en particulier avec pooling CLS, sont sous-optimaux en l’absence de fine-tuning.
- Le pooling par moyenne des jetons améliore la stabilité et les performances par rapport au pooling CLS.
- Le passage d’un classifieur linéaire (Logistic Regression) à un classifieur non linéaire (XGBoost) n’apporte qu’un gain marginal, ce qui indique que la principale limite provient de la représentation plutôt que du modèle de décision.
- Les visualisations UMAP montrent des amas sémantiques cohérents mais un fort chevauchement entre certaines classes médicales proches, ce qui explique les confusions observées.

---

## Temps d’exécution

⏱️ **Temps d’exécution total : ~20 à 50 minutes**

La parties la plus coûteuse est le calcul des embeddings Transformers  

---

## Dépendances

Les principales bibliothèques utilisées sont :
- numpy, pandas, scikit-learn  
- torch, transformers, sentence-transformers  
- umap-learn  
- matplotlib, seaborn  

Un fichier `requirements_NLP.txt` est fourni pour recréer l’environnement.

### Création de l’environnement Conda

```bash
conda create -n torch_NLP python=3.12
conda activate torch_NLP
pip install -r requirements_NLP.txt
```

---

## Reproductibilité

Pour garantir la reproductibilité :
- des graines aléatoires sont fixées pour NumPy, PyTorch et scikit-learn  
- les séparations train / test sont figées  
- les hyperparamètres des modèles sont conservés constants lors des comparaisons

---

## Remarques méthodologiques importantes

- Les visualisations UMAP et t-SNE sont utilisées uniquement à des fins exploratoires et qualitatives.  
  Elles ne constituent pas une preuve quantitative de séparabilité des classes.
- Les comparaisons entre modèles sont effectuées à représentation fixe et métrique fixe (F1 macro).
- Les résultats négatifs (absence de gain avec XGBoost) sont volontairement conservés et discutés.

---

## Conclusion

Ce notebook met en évidence que, pour des tâches de classification sémantique fines sur des textes médicaux courts :
- la qualité de la représentation est le principal facteur limitant,
- les embeddings de phrases entraînés en contrastif sont nettement plus adaptés que les embeddings génériques de type MLM,
- les classifieurs plus complexes ne compensent pas un manque de séparabilité sémantique intrinsèque.

---

## Auteur

Baptiste Granier  
IODAA - AgroParisTech et AMI2B – Université Paris-Saclay  
