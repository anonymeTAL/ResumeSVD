# ResumeSVD

**Ce code fonctionne avec Python 3.X**.

## Pour exécuter les expériences comme dans l'article, nous avons construit un script facile à utiliser :

1. Télécharger via le google drive les jeux de données, l'intégration des mots et les paramètres : [Drive folder](https://drive.google.com/drive/folders/1QjobC4w9G7nd2eva5sURUQ5Ys3s93gan?usp=sharing)  
**Vous devez garder la même structure de fichier**, ce qui signifie que vous devez mettre chaque fichier/dossier comme dans le google drive dans un dossier appelé ResumeSVD.

2. Téléchargez le fichier github ResumeSVD.py, et placez-le à la **racine du dossier ResumeSVD**.

3. Vous devez avoir les paquets des requirements.txt avec au moins la même version sur les paquets les plus importants qui sont : **gensim==3.8.3 / scipy==1.6.3 / rouge-score==0.0.4 / sklearn==0.17.2 / nltk==3.5 / numpy==1.19.2**.

4. Vous pouvez maintenant exécuter les expériences à partir de la racine de votre dossier ResumeSVD comme suit :  
```
python ResumeSVD.py --dataset_name "dataset_name" --size_output 300  
```
**Avec nom_de_données, l'un des éléments suivants :** "duc", "reddit", "cnn", "reddit_tifu", "MN" (pour Multi-News), "pubmed", "xsum". (size_output 0 signifie que nous testons l'ensemble de données complet. Pour exécuter n exemples, il suffit de mettre n à la place).

**Etape 5** : Comme vous le verrez, les résumés de sortie sont écrits dans le dossier de sortie comme : "nom_de_l'ensemble_de_données.txt". A partir de là, vous pouvez exécuter notre Rouge scorer en utilisant : 
```
python ResumeSVD.py --dataset_name "dataset_name" --scoring True
```
Note : Vous pouvez également noter vos propres résumés en remplaçant "nom_de_la_donnée" par le nom de votre fichier .txt dans le dossier "output".


## Temps de calcul de TextRank contre ResumeSVD sur un texte très long :    
Après avoir vu les résultats présentés dans l'article, on peut se demander comment notre méthode se compare à TextRank sur des documents très longs.
Nous réalisons cette comparaison en concaténant un nombre X de documents de n'importe quel ensemble de données, et en exécutant les deux méthodes sur ce nouveau document très long.
Vous pouvez exécuter l'expérience pour chaque jeu de données en utilisant ce qui suit : 

```
python ResumeSVD.py --dataset_name "dataset_name" --size_output 200 --longDocument "resumSVD"
```
Notez que --longDocument "ResumeSVD" lance l'expérience pour resumeSVD, puis vous pouvez la lancer pour TextRank en utilisant : 
```
python ResumeSVD.py --dataset_name "dataset_name" --size_output 200 --longDocument "textrank"
```
Nous avons obtenu les temps suivants en utilisant CNN et la concaténation de 200/500/1000/10000 documents (notez que les résultats peuvent varier en fonction de votre système) : 

| Nombre de documents concaténés (CNN) | TextRank | ResumeSVD | 
|---|---|---| 
| 200 | 44s | 45s | 
| 500 | 301s | 132s |  
| 1000 | 1219s | 284s | 
| 10000 | +20h | 4731s |


## Fonctions expérimentales :

Pour exécuter le jeu de données complet, pour size_output mettre la valeur 0, une valeur personnalisée prendra les n premiers documents et abstracts du jeu de données.

Si vous voulez essayer un jeu de données personnalisé, vous devrez l'appeler : nom_du_document + "_documents.pkl" et le gold : nom_du_document + "_gold.pkl". Vous pourrez ensuite l'exécuter avec :  
```
--dataset_name "dataset_name" --nb_sentences n --heuristic_rate xxx  
```
heuristic_rate (optionnel) signifie xxx * len(documents) pour choisir automatiquement les paramètres. Par défaut, il est fixé à 0.004. Arrangez-vous pour avoir au moins 15 exemples ou plus afin d'obtenir les meilleurs résultats.  
Les données .pkl doivent fonctionner avec :  
```
documents = pickle.load(open(dataset_name + "_documents.pkl", "rb"))
gold = pickle.load(open(nom_de_données + "_gold.pkl", "rb"))
```

Si vous voulez essayer une intégration de mots personnalisée, elle doit être au format .pkl. Elle doit fonctionner comme suit : wordembedding["word"] => vector.  
Dans ce cas, ajoutez simplement :  
```
--word_embedding_path "./Word Embedding Model/my_word_embedding.pkl"
```

Si vous souhaitez uniquement utiliser notre évaluation Rouge, vous devrez utiliser : python ResumeSVD.py --dataset_name "dataset_name" --scoring True  
Avec le jeu de données ajouté comme nom_de_données + "_documents.pkl" et le gold : nom_de_données + "_gold.pkl" dans le dossier Datasets.  
Vos résumés devront être dans le dossier de sortie comme "nom_documents.txt".

Nous présentons ici la méthode associée à la sortie faite du même nombre de phrases annoncées dans l'article.  
De meilleurs résultats peuvent être obtenus avec un nombre différent de phrases, désigné par "n". 
    
     
      
     


### Questions, problèmes, commentaires ? Faites-nous en part !
  
