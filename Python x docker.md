**Directory structure for a python web app**

├── Dockerfile  (1)

├── docker-compose.yml  (1)

├── requirements.txt

├── Makefile  (2)

├── README.md

├── license.txt

├── mypy.ini

├── src  (3)
│   ├── allocation
│   │   ├── __init__.py
│   │   ├── adapters
│   │   │   ├── __init__.py
│   │   │   ├── orm.py
│   │   │   └── repository.py
│   │   ├── config.py
│   │   ├── domain
│   │   │   ├── __init__.py
│   │   │   └── model.py
│   │   ├── entrypoints
│   │   │   ├── __init__.py
│   │   │   └── flask_app.py
│   │   └── service_layer
│   │       ├── __init__.py
│   │       └── services.py
│   └── setup.py  (3)
└── tests  (4)
    ├── conftest.py  (4)
    ├── e2e
    │   └── test_api.py
    ├── integration
    │   ├── test_orm.py
    │   └── test_repository.py
    ├── pytest.ini  (4)
    └── unit
        ├── test_allocate.py
        ├── test_batches.py
        └── test_services.p
