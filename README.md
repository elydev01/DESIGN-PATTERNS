# DESIGN PATTERNS (WITH PYTHON)

Liste des 10 modèles de conception couramment utilisés en informatique pour résoudre des problèmes récurrents dans le développement de logiciels.

Ils sont souvent considérés comme des solutions éprouvées et standardisées pour résoudre des problèmes de conception communs.

Chaque pattern a un objectif et une utilisation spécifiques.
Par exemple, le modèle Singleton est utilisé pour s'assurer qu'il n'y a qu'une seule instance d'une classe dans toute l'application, tandis que le modèle Observateur permet de mettre en place un système d'événements pour notifier des objets lorsqu'un autre objet subit des changements.

En utilisant ces modèles de conception, les développeurs peuvent améliorer l'efficacité de leur code et réduire les erreurs de conception.
Cependant, il est important de comprendre que chaque pattern doit être utilisé de manière appropriée, car une mauvaise utilisation peut rendre le code plus complexe et difficile à maintenir.

## 1. Modèle Singleton

**Singleton**

### Description:

Le modèle Singleton garantit qu'une seule instance d'une classe sera créée et fournit un point d'accès global à cette instance. Cela est utile dans les situations où une seule instance doit être utilisée dans toute l'application, par exemple pour des ressources partagées ou des configurations globales.

### Exemple de code:

```Python
class Singleton:
    _instance = None

    def __new__(cls):
        if cls._instance is None:
            cls._instance = super().__new__(cls)
        return cls._instance

# Utilisation du modèle Singleton pour créer une instance unique d'une classe
s1 = Singleton()
s2 = Singleton()

print(s1 is s2) # True
```

Dans cet exemple, la classe `Singleton` utilise la méthode `__new__` pour créer une seule instance de la classe en mémoire. La variable de classe `_instance` est utilisée pour stocker l'instance unique, et la méthode `__new__` vérifie si cette variable est nulle avant de créer une nouvelle instance. Si l'instance existe déjà, la méthode `__new__` renvoie simplement cette instance plutôt que d'en créer une nouvelle. Cela garantit que seule une instance de la classe `Singleton` sera créée pendant toute la durée de l'exécution du programme.

> Note: Il existe d'autres méthodes pour implémenter le modèle Singleton en Python, notamment en utilisant un décorateur ou un métaclasse.

## 2. Modèle Factory

**Factory**

### Description:

Le modèle Factory permet de déléguer la création d'objets à une classe distincte, appelée "Factory". Cela permet de séparer la création d'objets de leur utilisation, ce qui peut rendre le code plus lisible et plus facile à maintenir. Le modèle Factory est souvent utilisé pour créer des objets en fonction de certains paramètres ou conditions.

### Exemple de code:

```Python
class Animal:
    def __init__(self, name):
        self.name = name
    
    def speak(self):
        pass

class Dog(Animal):
    def speak(self):
        return "Woof!"

class Cat(Animal):
    def speak(self):
        return "Meow!"

class AnimalFactory:
    def create_animal(self, kind, name):
        if kind == "dog":
            return Dog(name)
        elif kind == "cat":
            return Cat(name)
        else:
            raise ValueError(f"Invalid animal kind: {kind}")

# Utilisation de la Factory pour créer des animaux
factory = AnimalFactory()
dog = factory.create_animal("dog", "Rex")
cat = factory.create_animal("cat", "Kitty")

print(dog.speak()) # "Woof!"
print(cat.speak()) # "Meow!"
```

Dans cet exemple, la classe `AnimalFactory` est utilisée pour créer des objets `Animal` en fonction de leur type (`"dog"` ou `"cat"`) et de leur nom. La méthode `create_animal` prend ces paramètres en entrée et renvoie l'objet approprié. De cette manière, la création d'objets est déléguée à la Factory et peut être facilement étendue ou modifiée à l'avenir.

## 3. Modèle Observateur

**Observateur (ou Observer)**

### Description:

Le modèle Observateur permet de mettre en place un système d'événements où des objets sont notifiés lorsqu'un autre objet subit des changements. Ce modèle est souvent utilisé pour implémenter des interfaces utilisateur réactives ou des systèmes de notification en temps réel.

### Exemple de code:

```Python
class Observable:
    def __init__(self):
        self._observers = []

    def register_observer(self, observer):
        self._observers.append(observer)

    def unregister_observer(self, observer):
        self._observers.remove(observer)

    def notify_observers(self):
        for observer in self._observers:
            observer.notify(self)

class Observer:
    def notify(self, observable):
        pass

class Button(Observable):
    def __init__(self):
        super().__init__()
        self._label = "Click me"

    def click(self):
        self.notify_observers()

    @property
    def label(self):
        return self._label

    @label.setter
    def label(self, value):
        self._label = value
        self.notify_observers()

class Label(Observer):
    def __init__(self, text):
        self._text = text

    def notify(self, observable):
        if isinstance(observable, Button):
            self._text = observable.label

    @property
    def text(self):
        return self._text

# Utilisation du modèle Observateur pour mettre à jour une étiquette en fonction du texte d'un bouton
button = Button()
label = Label(button.label)
button.register_observer(label)

print(label.text) # "Click me"
button.label = "Press me"
print(label.text) # "Press me"
```

Dans cet exemple, la classe `Observable` définit les méthodes pour ajouter, supprimer et notifier des observateurs. La classe `Observer` définit la méthode `notify` qui sera appelée par l'observable lorsqu'un changement survient. Les classes `Button` et `Label` héritent de ces classes de base et implémentent leur propre logique métier. Lorsqu'un bouton est cliqué ou que son étiquette est mise à jour, il notifie tous ses observateurs, y compris l'étiquette, qui met à jour son propre texte en conséquence.

## 4. Modèle Stratégie

**Stratégie (ou Strategy)**

### Description:

Le modèle Stratégie permet de définir une famille d'algorithmes et de les encapsuler de manière à ce qu'ils soient interchangeables. Cela permet de modifier le comportement d'un objet en temps d'exécution en utilisant différents algorithmes sans modifier la classe de cet objet.

### Exemple de code:

```Python
class Strategy:
    def execute(self, data):
        pass

class Algorithm1(Strategy):
    def execute(self, data):
        return data * 2

class Algorithm2(Strategy):
    def execute(self, data):
        return data + 1

class Context:
    def __init__(self, strategy):
        self._strategy = strategy

    def execute_strategy(self, data):
        return self._strategy.execute(data)

# Utilisation du modèle Stratégie pour utiliser différents algorithmes de manière interchangeable
context1 = Context(Algorithm1())
result1 = context1.execute_strategy(10) # 20

context2 = Context(Algorithm2())
result2 = context2.execute_strategy(10) # 11
```

Dans cet exemple, la classe `Strategy` définit une interface pour les différents algorithmes, et les classes `Algorithm1` et `Algorithm2` implémentent cette interface avec leur propre logique métier. La classe `Context` utilise une instance de l'un des algorithmes pour effectuer une opération sur des données en appelant la méthode `execute` de l'algorithme. En modifiant l'instance de la classe `Context` avec un autre algorithme, il est possible d'appliquer un comportement différent à la même opération.

## 5. Modèle Commande

**Commande (ou Command)**

### Description:

Le modèle Commande permet de définir une interface pour encapsuler une requête en tant qu'objet, ce qui permet de paramétrer des méthodes avec différentes requêtes. Cela permet de séparer la création de la requête de son exécution, de gérer les requêtes en tant qu'objets et de permettre l'annulation et la répétition des requêtes.

### Exemple de code:

```Python
class Command:
    def execute(self):
        pass

class Light:
    def on(self):
        print("La lumière est allumée.")

    def off(self):
        print("La lumière est éteinte.")

class LightOnCommand(Command):
    def __init__(self, light):
        self._light = light

    def execute(self):
        self._light.on()

class LightOffCommand(Command):
    def __init__(self, light):
        self._light = light

    def execute(self):
        self._light.off()

class RemoteControl:
    def __init__(self):
        self._on_commands = []
        self._off_commands = []

    def set_command(self, slot, on_command, off_command):
        self._on_commands.insert(slot, on_command)
        self._off_commands.insert(slot, off_command)

    def press_on_button(self, slot):
        self._on_commands[slot].execute()

    def press_off_button(self, slot):
        self._off_commands[slot].execute()

# Utilisation du modèle Commande pour encapsuler des requêtes et les paramétrer avec différentes méthodes
remote_control = RemoteControl()

light = Light()
light_on_command = LightOnCommand(light)
light_off_command = LightOffCommand(light)

remote_control.set_command(0, light_on_command, light_off_command)

remote_control.press_on_button(0) # Allume la lumière
remote_control.press_off_button(0) # Éteint la lumière
```

Dans cet exemple, la classe `Command` définit une interface pour les commandes. Les classes `LightOnCommand` et `LightOffCommand` implémentent cette interface pour allumer et éteindre une lumière. La classe `RemoteControl` utilise des instances des classes de commandes pour exécuter des requêtes à partir de boutons de la télécommande en appelant les méthodes `execute` des commandes. En utilisant le modèle Commande, il est possible d'encapsuler des requêtes en tant qu'objets et de les paramétrer avec différentes méthodes, ce qui permet une plus grande flexibilité et un meilleur contrôle des actions.

## 6. Modèle Décorateur

**Décorateur (Decorator)**

### Description:

Le modèle Décorateur permet d'ajouter des fonctionnalités à une classe existante, sans avoir à la modifier directement. Cela permet d'étendre les capacités d'un objet sans impacter les autres objets de la même classe. Le modèle Décorateur est utile lorsque l'on veut ajouter des fonctionnalités à un objet de manière dynamique, sans avoir besoin de recréer l'objet.

### Exemple de code:

```Python
class Component:
    def operation(self) -> str:
        pass

class ConcreteComponent(Component):
    def operation(self) -> str:
        return "ConcreteComponent"

class Decorator(Component):
    _component: Component = None

    def __init__(self, component: Component) -> None:
        self._component = component

    @property
    def component(self) -> str:
        return self._component

    def operation(self) -> str:
        return self._component.operation()

class ConcreteDecoratorA(Decorator):
    def operation(self) -> str:
        return f"ConcreteDecoratorA({self.component.operation()})"

class ConcreteDecoratorB(Decorator):
    def operation(self) -> str:
        return f"ConcreteDecoratorB({self.component.operation()})"
```

Dans cet exemple, la classe `Component` définit une méthode `operation` qui sera implémentée par les classes concrètes. La classe `ConcreteComponent` implémente cette méthode de manière simple, sans ajouter de fonctionnalités supplémentaires.

Les classes `Decorator` et `ConcreteDecorator` permettent d'ajouter des fonctionnalités à `ConcreteComponent`. `Decorator` est une classe abstraite qui contient une instance de `Component`, tandis que `ConcreteDecoratorA` et `ConcreteDecoratorB` ajoutent des fonctionnalités supplémentaires à cette instance.

En utilisant le modèle Décorateur, on peut ajouter des fonctionnalités à un objet de manière dynamique, sans avoir besoin de créer une nouvelle classe pour chaque combinaison de fonctionnalités.

## 7. Modèle Adaptateur

**Adaptateur (Adapter)**

### Description:

Le modèle de conception "Adaptateur" permet d'adapter une interface existante à une nouvelle interface, afin de rendre compatibles des objets qui ne le seraient pas autrement. Il est utile lorsque vous devez utiliser un objet existant dans une nouvelle application mais que son interface ne correspond pas à celle de l'application. Au lieu de modifier l'objet existant, vous pouvez créer un adaptateur qui agit comme une couche intermédiaire entre l'objet existant et l'application. L'adaptateur traduit les appels de méthode de l'application en appels de méthode compréhensibles pour l'objet existant.

### Exemple de code:

Voici un exemple de code pour le modèle de conception "Adaptateur" en Python. Dans cet exemple, nous avons une interface "Document" qui est utilisée par une application. Nous avons également une classe "PDFDocument" qui implémente l'interface "Document", mais qui n'est pas compatible avec l'application. Pour résoudre ce problème, nous créons une classe "PDFDocumentAdapter" qui implémente l'interface "Document" de l'application, mais qui utilise l'objet "PDFDocument" comme couche intermédiaire.

```Python
class Document:
    def open(self):
        pass

    def close(self):
        pass

class PDFDocument:
    def open_pdf(self):
        print("Ouverture du PDF")

    def close_pdf(self):
        print("Fermeture du PDF")

class PDFDocumentAdapter(Document):
    def __init__(self):
        self.pdf = PDFDocument()

    def open(self):
        self.pdf.open_pdf()

    def close(self):
        self.pdf.close_pdf()

# Utilisation de l'adaptateur pour ouvrir et fermer un document PDF
document = PDFDocumentAdapter()
document.open()
document.close()
```

Dans cet exemple, nous créons une classe "PDFDocumentAdapter" qui hérite de l'interface "Document" et utilise une instance de "PDFDocument" pour effectuer les opérations "open" et "close". Cela permet à l'objet "PDFDocument" d'être utilisé dans l'application sans avoir besoin de le modifier pour correspondre à l'interface de l'application.

## 8. Modèle Proxy

**Proxy (ou Procuration en français)**

### Description:

le modèle de conception Proxy permet de contrôler l'accès à un objet en créant un objet intermédiaire qui agit comme un substitut. Le Proxy peut être utilisé pour différentes raisons, comme le contrôle de l'accès à un objet distant, la création paresseuse d'un objet coûteux en ressources, la mise en cache des résultats d'une opération, etc.

### Exemple de code:

```Python
from abc import ABC, abstractmethod

class Sujet(ABC):
    """
    Interface du sujet que le Proxy et le Sujet concret doivent implémenter.
    """

    @abstractmethod
    def operation(self):
        pass

class SujetConcret(Sujet):
    """
    Sujet concret qui implémente l'interface Sujet.
    """

    def operation(self):
        print("Opération réalisée par le sujet concret.")

class Proxy(Sujet):
    """
    Proxy qui contrôle l'accès au sujet concret.
    """

    def __init__(self):
        self._sujet_concret = None

    def operation(self):
        if self._sujet_concret is None:
            self._sujet_concret = SujetConcret()
        print("Opération réalisée par le proxy avant d'appeler le sujet concret.")
        self._sujet_concret.operation()

# Exemple d'utilisation du Proxy
proxy = Proxy()
proxy.operation()
```

Dans cet exemple, on crée une classe `Sujet` qui représente l'interface que doivent implémenter le Proxy et le Sujet concret. On crée ensuite une classe `SujetConcret` qui implémente cette interface et qui représente l'objet que le Proxy doit contrôler l'accès. Enfin, on crée la classe `Proxy` qui agit comme un substitut de `SujetConcret` et qui contrôle l'accès à cet objet. Lorsque la méthode `operation` est appelée sur le Proxy, celui-ci réalise d'abord une opération préliminaire avant d'appeler la méthode `operation` sur le Sujet concret.

## 9. Modèle Composite

**Composite**

### Description:

Le modèle Composite est un modèle de conception structurel qui permet de regrouper des objets en une structure arborescente, de manière à traiter ces objets de manière uniforme. Le modèle Composite utilise une hiérarchie de classes pour représenter les objets individuels ainsi que les groupes d'objets. Cela permet de traiter une collection d'objets de manière identique à un seul objet.

Le modèle Composite est utile lorsque vous devez traiter une collection d'objets de manière identique à un seul objet. Cela peut être particulièrement utile lors de la manipulation d'objets dans une structure arborescente, comme une interface graphique utilisateur. Le modèle Composite permet de traiter les nœuds de l'arbre et les feuilles de manière identique.

### Exemple de code:

```Python
class Component:
    """
    Classe de base pour les composants
    """
    def __init__(self, name):
        self.name = name
    
    def add(self, component):
        pass
    
    def remove(self, component):
        pass
    
    def display(self):
        pass


class Leaf(Component):
    """
    Classe pour les feuilles de l'arbre
    """
    def display(self):
        print(self.name)


class Composite(Component):
    """
    Classe pour les nœuds de l'arbre
    """
    def __init__(self, name):
        super().__init__(name)
        self.children = []
    
    def add(self, component):
        self.children.append(component)
    
    def remove(self, component):
        self.children.remove(component)
    
    def display(self):
        print(self.name)
        for child in self.children:
            child.display())


# Utilisation du modèle Composite
root = Composite("Root")
node1 = Composite("Node 1")
node2 = Composite("Node 2")
leaf1 = Leaf("Leaf 1")
leaf2 = Leaf("Leaf 2")
leaf3 = Leaf("Leaf 3")

root.add(node1)
root.add(node2)
node1.add(leaf1)
node2.add(leaf2)
node2.add(leaf3)

root.display()
```

Dans cet exemple, le modèle Composite est utilisé pour représenter une structure arborescente de composants. La classe `Component` est une classe de base qui définit les méthodes communes à toutes les classes de composants. La classe `Leaf` représente les feuilles de l'arbre, tandis que la classe `Composite` représente les nœuds de l'arbre. La méthode `display()` est définie pour afficher les noms de tous les composants dans l'arbre.

## 10. Modèle État

**État (State)**

### Description:

Le modèle de conception État permet de changer le comportement d'un objet en fonction de son état interne, en encapsulant chaque état dans une classe distincte.

Le modèle État est utile lorsque les objets ont des comportements différents en fonction de leur état interne, et que ces comportements changent dynamiquement en réponse à des événements. Ce modèle permet d'encapsuler chaque état dans une classe distincte, et de définir des méthodes pour chaque transition entre états.

### Exemple de code:

Dans l'exemple suivant, nous avons une classe GumballMachine qui vend des bonbons. La machine peut avoir plusieurs états différents (par exemple, "pas de pièce insérée", "pièce insérée", "bonbon vendu", etc.). Nous utilisons le modèle État pour encapsuler chaque état dans une classe distincte, et pour définir des méthodes pour chaque transition entre états.

```Python
from abc import ABC, abstractmethod

class GumballState(ABC):
    @abstractmethod
    def insert_coin(self):
        pass
    
    @abstractmethod
    def eject_coin(self):
        pass
    
    @abstractmethod
    def turn_crank(self):
        pass
    
    @abstractmethod
    def dispense(self):
        pass
    

class NoCoinState(GumballState):
    def insert_coin(self):
        print("Vous avez inséré une pièce")
        return HasCoinState()
    
    def eject_coin(self):
        print("Il n'y a pas de pièce à éjecter")
        return self
    
    def turn_crank(self):
        print("Veuillez d'abord insérer une pièce")
        return self
    
    def dispense(self):
        print("Veuillez d'abord tourner la manivelle")
        return self
    

class HasCoinState(GumballState):
    def insert_coin(self):
        print("Vous avez déjà inséré une pièce")
        return self
    
    def eject_coin(self):
        print("Pièce éjectée")
        return NoCoinState()
    
    def turn_crank(self):
        print("Vous avez tourné la manivelle")
        return SoldState()
    
    def dispense(self):
        print("Veuillez d'abord tourner la manivelle")
        return self
    

class SoldState(GumballState):
    def insert_coin(self):
        print("Attendez s'il vous plaît, nous vous donnons déjà un bonbon")
        return self
    
    def eject_coin(self):
        print("Désolé, vous avez déjà tourné la manivelle")
        return self
    
    def turn_crank(self):
        print("Vous avez déjà tourné la manivelle")
        return self
    
    def dispense(self):
        print("Un bonbon sort de la machine")
        return NoCoinState()

        
class GumballMachine:
    def __init__(self):
        self._state = NoCoinState()
        
    def set_state(self, state):
        self._state = state
        
    def insert_coin(self):
        self.set_state(self._state.insert_coin())
        
    def eject_coin(self):
        self.set_state(self._state.eject_coin())
        
    def turn_crank(self):
        self.set_state(self._state.turn_crank())
        self.set_state(self._state.dispense())
        
    def refill(self):
        pass
```

Dans cet exemple, nous avons une classe abstraite `State` qui définit une interface commune pour tous les états possibles. Chaque état est représenté par une classe distincte qui hérite de la classe `State`.

Le contexte de l'état, représenté par la classe `Context`, maintient une référence à l'état courant, et permet de changer l'état courant en appelant la méthode `change_state()`.

> Voici un exemple de code pour le modèle État:

```Python
from abc import ABC, abstractmethod

class State(ABC):
    @abstractmethod
    def handle(self):
        pass

class EtatA(State):
    def handle(self):
        print("Traitement de l'état A")
        self.context.change_state(EtatB())

class EtatB(State):
    def handle(self):
        print("Traitement de l'état B")
        self.context.change_state(EtatA())

class Context:
    def __init__(self):
        self.etat_courant = EtatA()
        self.etat_courant.context = self

    def change_state(self, nouvel_etat):
        self.etat_courant = nouvel_etat
        self.etat_courant.context = self

    def request(self):
        self.etat_courant.handle()

# Utilisation
context = Context()
context.request()  # Output: Traitement de l'état A
context.request()  # Output: Traitement de l'état B
context.request()  # Output: Traitement de l'état A
```

Dans cet exemple, la classe abstraite `State` définit une méthode abstraite `handle()` qui doit être implémentée par chaque classe d'état. Chaque classe d'état représente un état différent, et implémente la méthode `handle()` pour effectuer les actions appropriées.

La classe `Context` maintient une référence à l'état courant, et fournit une méthode `change_state()` pour changer l'état courant en fonction des besoins. La méthode `request()` est utilisée pour appeler la méthode `handle()` de l'état courant, ce qui déclenche le traitement approprié en fonction de l'état courant.

Dans cet exemple, le modèle État est utilisé pour gérer le comportement d'un objet en fonction de son état interne. Chaque état est représenté par une classe distincte, ce qui permet de séparer la logique de chaque état en modules distincts et faciles à maintenir.
