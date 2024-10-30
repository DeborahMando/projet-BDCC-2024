Voici une explication détaillée des commandes MongoDB sous forme Markdown en français :

```markdown
# Requêtes et Opérations MongoDB

## Sélection de la Base de Données et Requêtes de Base

1. **Changer de base de données** :
   ```javascript
   db = db.getSiblingDB("new_york");
   ```
   - Cette commande permet de passer à la base de données `new_york`.

2. **Obtenir tous les documents de la collection `restaurants`** :
   ```javascript
   db.getCollection("restaurants").find({});
   ```
   - Récupère tous les documents de la collection `restaurants`.

3. **Trouver un document** :
   ```javascript
   db.restaurants.findOne();
   ```
   - Renvoie un seul document de la collection `restaurants`.

4. **Trouver les documents situés à Brooklyn** :
   ```javascript
   db.restaurants.find({ "borough": "Brooklyn" });
   ```
   - Recherche tous les restaurants situés dans le quartier de Brooklyn.

5. **Compter les documents à Brooklyn** :
   ```javascript
   db.restaurants.find({ "borough": "Brooklyn" }).count();
   ```
   - Renvoie le nombre de restaurants situés à Brooklyn.

6. **Trouver les restaurants italiens à Brooklyn** :
   ```javascript
   db.restaurants.find({ "borough": "Brooklyn", "cuisine": "Italian" });
   ```
   - Trouve les restaurants italiens à Brooklyn.

7. **Compter les restaurants italiens à Brooklyn** :
   ```javascript
   db.restaurants.find({ "borough": "Brooklyn", "cuisine": "Italian" }).count();
   ```
   - Compte les restaurants italiens situés à Brooklyn.

8. **Trouver les restaurants italiens sur l'Avenue 5 à Brooklyn** :
   ```javascript
   db.restaurants.find({ "borough": "Brooklyn", "cuisine": "Italian", "address.street": "5 Avenue" });
   ```

9. **Compter les restaurants italiens sur l'Avenue 5 à Brooklyn** :
   ```javascript
   db.restaurants.find({ "borough": "Brooklyn", "cuisine": "Italian", "address.street": "5 Avenue" }).count();
   ```

## Recherche avec Expressions Régulières et Projection de Champs

1. **Trouver les restaurants italiens sur l'Avenue 5 avec "pizza" dans le nom** :
   ```javascript
   db.restaurants.find({ "borough": "Brooklyn", "cuisine": "Italian", "address.street": "5 Avenue", "name": /pizza/i });
   ```
   - Trouve les restaurants italiens situés sur l'Avenue 5 à Brooklyn, avec "pizza" dans le nom (insensible à la casse).

2. **Projeter uniquement le champ `name`** :
   ```javascript
   db.restaurants.find({ "borough": "Brooklyn", "cuisine": "Italian", "address.street": "5 Avenue", "name": /pizza/i }, { "name": 1 });
   ```

3. **Projeter les champs `name` et `grades`** :
   ```javascript
   db.restaurants.find({ "borough": "Brooklyn", "cuisine": "Italian", "address.street": "5 Avenue", "name": /pizza/i }, { "name": 1, "grades.score": 1 });
   ```

## Requêtes avec Opérateurs Conditionnels

1. **Trouver les restaurants avec un score bas à Manhattan** :
   ```javascript
   db.restaurants.find({ "borough": "Manhattan", "grades.score": { $lt: 10 }}, { "name": 1, "grades.score": 1, "_id": 0 });
   ```

2. **Trouver les restaurants avec une note C et un score < 30** :
   ```javascript
   db.restaurants.find({ "grades.grade": "C", "grades.score": { $lt: 30 }}, { "grades.grade": 1, "grades.score": 1 });
   ```

## Opérations avec le Pipeline d'Agrégation

1. **Match et Projection** :
   ```javascript
   var varMatch = { $match: { "grades.0.grade": "C" } };
   var varProject = { $project: { "name": 1, "borough": 1, "_id": 0 } };
   db.restaurants.aggregate([varMatch, varProject]);
   ```

2. **Opération de Tri** :
   ```javascript
   var varSort = { $sort: { "name": 1 } };
   db.restaurants.aggregate([varMatch, varProject, varSort]);
   ```

3. **Opérations de Groupement** :
   ```javascript
   var varGroup = { $group: { "_id": null, "total": { $sum: 1 } } };
   db.restaurants.aggregate([varMatch, varGroup]);
   ```

4. **Calcul de la Moyenne des Scores par Borough** :
   ```javascript
   var varUnwind = { $unwind: "$grades" };
   var varGroup4 = { $group: { "_id": "$borough", "moyenne": { $avg: "$grades.score" } } };
   var varSort2 = { $sort: { "moyenne": -1 } };
   db.restaurants.aggregate([varUnwind, varGroup4, varSort2]);
   ```

## Opérations de Mise à Jour

1. **Mettre à jour un document avec un commentaire** :
   ```javascript
   db.restaurants.update({"_id": ObjectID("594b9172c96c61e672dcd689")}, { $set: { "comment": "Mon nouveau commentaire" } });
   ```

2. **Retirer le champ `comment`** :
   ```javascript
   db.restaurants.update({"_id": ObjectID("594b9172c96c61e672dcd689")}, { $unset: { "comment": 1 } });
   ```

3. **Ajouter un commentaire pour les restaurants sans note C** :
   ```javascript
   db.restaurants.update({ "grades.grade": { $not: { $eq: "C" } } }, { $set: { "comment": "acceptable" } }, { "multi": true });
   ```

Ces commandes illustrent divers types d'opérations MongoDB, y compris des requêtes, des agrégations et des mises à jour de champs, en se focalisant spécifiquement sur des données de restaurants triées par borough, cuisine et scores de grade.
```