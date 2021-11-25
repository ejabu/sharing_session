# Open x Close Principle

## Overview

| Poin-poin penting                                                                              |
| ---------------------------------------------------------------------------------------------- |
| Software entities(Classes, modules, functions) should be OPEN for extension, CLOSED for modification. |
| Banyak yang nulis Open / Close Principle, tapi rancu seperti satu `Main Idea`, padahal dua     |

## Reference

[Github](https://github.com/heykarimoff/solid.python/blob/master/2.ocp.py)

## Contoh 1

=== ":octicons-circle-slash-16: Bad"

    _Contoh_:

    ````python linenums="1"
    class Animal:
        def __init__(self, name: str):
            self.name = name

        def get_name(self) -> str:
            pass


    def animal_sound(animals: list):
        for animal in animals:
            if animal.name == 'lion':
                print('roar')

            elif animal.name == 'mouse':
                print('squeak')


    def client_code():
        animals = [
            Animal('lion'),
            Animal('mouse')
        ]
        animal_sound(animals)
    ````

=== ":octicons-check-circle-fill-16: Good"

    ```python
    class Animal:
        def __init__(self, name: str):
            self.name = name

        def get_name(self) -> str:
            pass

        def make_sound(self):
            pass


    class Lion(Animal):
        def make_sound(self):
            return 'roar'


    class Mouse(Animal):
        def make_sound(self):
            return 'squeak'


    class Cat(Animal):
        def make_sound(self):
            return 'meow'


    def client_code():
        animals = [
            Lion('King'),
            Mouse('Jerry'),
            Cat('Garfield'),
        ]
        for animal in animals:
            sound = animal.make_sound()
            print(sound)
    ```
