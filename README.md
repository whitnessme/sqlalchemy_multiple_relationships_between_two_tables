# SQLAlchemy Multiple Relationships Between Two Tables

> After having difficulty with SQLAlchemy docs on how to get this to work, I finally got assistance to get this correct. So here is this resource to connect a model to another model two times. *In the future I will put in instructions for other types.*

 ## The 'Two to One' Scenario:
 For my flashcard project I had Users and Decks models that I needed to connect twice. A deck has an **owner** who created it but, if shared, other users can "download" that deck to use and change. I wanted to display who originally created the deck and still have another user who owned this copy of the deck. 
 
 ![user and deck database schema tables](https://user-images.githubusercontent.com/89945390/177868109-83110247-93f3-4ff6-bdb1-e31299912977.png)

### The relationships in the *Deck* Model:
```
user = db.relationship("User",
       foreign_keys=[user_id],
       back_populates="deck_user")

owner = db.relationship("User",
        foreign_keys=[owner_id],
        back_populates="deck_owner")
```
### The relationships in the *User* Model:
```
deck_user = db.relationship("Deck",
            foreign_keys='Deck.user_id',
            back_populates="user",
            lazy='dynamic')

deck_owner = db.relationship("Deck",
             foreign_keys='Deck.owner_id',
             back_populates="owner",
             lazy='dynamic')
```
### In the `to_dict` method for *Deck* model:
```
def  to_dict(self):
     return  {
          "id":  self.id,
          "owner_id":  self.owner_id,
          "user_id":  self.user_id,
          ...
          # code removed for brevity
          }
```
-------
