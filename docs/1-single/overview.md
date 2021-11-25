# Single Responsibility

## Overview

| Poin-poin penting                                                | Keterangan                                                                                                            |
| ---------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------- |
| Satu pekerjaan dalam satu `Class`                                |
| **Suatu class tidak diperbolehkan untuk mengerjakan banyak hal** | Jika di dalam class invoice terdapat fungsi untuk mencetak invoice, akses basis data, dan melakukan perhitungan order |
| Repository Pattern                                               | Terkenal semenjak DDD, 2004                                                                                           |

## Reference

[Medium.com](https://medium.com/@Dewey92/repository-pattern-what-e47ddee3364d), [Deviq.com](https://deviq.com/design-patterns/repository-pattern), [Facade Pattern](https://refactoring.guru/design-patterns/facade/python/example)

## Contoh 1

=== ":octicons-circle-slash-16: Bad"

    _Contoh_:

    ````python linenums="1"
    class Animal:
        def __init__(self, name: str):
            self.name = name

        def get_name(self) -> str:
            pass

        def save_to_db(self, animal: Animal):
            pass
    ````

=== ":octicons-check-circle-fill-16: Good"

    Kita harus buat `Class` baru yang fokus untuk CRUD ke Database

    _Contoh_:

    ````python linenums="1"
    class Animal:
        def __init__(self, name: str):
            self.name = name

        def get_name(self) -> str:
            pass


    class AnimalDB:
        def get_animal(self, id) -> Animal:
            pass

        def save_to_db(self, animal: Animal): # (1)
            pass
    ````

    1. Pindah kesini

    !!! info "Implementasi"


    ````python linenums="1"
    class Animal:
        def __init__(self, name: str):
            self.name = name

        def get_name(self) -> str:
            pass


    class AnimalDB:
        def get_animal(self, id) -> Animal:
            pass

        def save_to_db(self, animal: Animal):
            pass


    def client_code():
        kucing = Animal("Garfield")
        AnimalDB().save_to_db(kucing)


    ````

=== ":octicons-check-circle-fill-16: Better"

    Atau bisa kita tambahkan dengan [Facade Pattern](https://github.com/heykarimoff/solid.python/blob/d8a66834fc508f227617d28a9894873a488ae5e6/1.srp.py#L69)

    _Contoh_:

    ````python linenums="1"
    class Animal: # (1)
        def __init__(self, name: str):
            self.name = name
            self.db = AnimalDB()

        def get_name(self) -> str:
            return self.name

        def get(self, id):
            return self.db.get_animal(id)

        def save(self):
            self.db.save_to_db(animal=self)


    class AnimalDB:
        def get_animal(self, id) -> Animal:
            pass

        def save_to_db(self, animal: Animal): # (2)
            pass


    def client_code():
        kucing = Animal("Garfield")
        kucing.save()
    ````

    1. Facade Pattern

    2. Pindah kesini
