### Coding Rules

#### General
* All methods, classes, functions, and modules should have a docstring in Google format.
* Follow the PEP 8 style guide for Python code using flake8.
* Use unit tests to verify the correctness of your code and catch regressions by using pytest or unittest framework.
* Use version control (such as Git) to track changes to your code and collaborate with others.
* Use continuous integration (CI) and continuous deployment (CD) practices to automate the testing and deployment of your code.
* Keep your code modular and loosely coupled, as this can improve code reusability and maintainability.

#### Variables
* The names of variables should be long and human-readable, with no single-letter names.
* They should also be prefixed with their type, like this:
```python
str_my_fantastic_phrases = ["hello world", "goodbye moon"]
int_number_of_classes = 10
dict_of_my_clients_and_phone_number = {"Alice": "555-1234", "Bob": "555-5678"}
list_of_yellow_fruits = ["banana", "pineapple", "lemon"]
```

#### Functions
* Use descriptive, meaningful function and method names that clearly convey their purpose. For example, get\_user\_by\_id is a better name than get\_data.
* Use exception handling to gracefully handle errors and unexpected conditions in your code.
* The types of arguments should be included in the function signature.
* Here's an example of a function:
```python
def calculate_average(numbers: List[float]) -> float:
    """Calculates the average of a list of numbers.

    Args:
        numbers (List[float]): A list of floating-point numbers.

    Returns:
        float: The average of the numbers in the list.

    Raises:
        ValueError: If the list of numbers is empty.

    Example:
        >>> calculate_average([1.0, 2.0, 3.0])
        2.0
    """
    if not numbers:
        raise ValueError("The list of numbers cannot be empty.")

    return sum(numbers) / len(numbers)
```

#### Classes and Methods
* A logger should be passed to each class and initialized like this:
```python
import logging

class JsonFileHistoryExtractor:
    def __init__(self, logger: logging.Logger = None):
        """Extract history of measures from the same aircraft as the input JSON file in the last n months.
        """
        self.logger = logger or logging.getLogger(__name__)
```
* The organization of the code should adhere to the SOLID principles:
	+ **Single Responsibility Principle**: Each class or module should have a single, well-defined responsibility.
	+ **Open/Closed Principle**: The code should be open for extension (new features or functionality can be added) but closed for modification (the existing code doesn't need to change).
	+ **Liskov Substitution Principle**: Subtypes should be substitutable for their base types without affecting the correctness of the program.
	+ **Interface Segregation Principle**: The code should depend on narrowly-defined interfaces rather than broad, generic ones.
	+ **Dependency Inversion Principle**: The code should depend on abstractions (interfaces or abstract base classes) rather than concrete implementations.

### SOLID Principles Example

Here's an example of how the SOLID principles can be applied to a set of classes called in the `__init__` method of a manager class:
```python
from abc import ABC, abstractmethod
import logging

class DataProvider(ABC):
    @abstractmethod
    def get_data(self) -> str:
        pass

class FileDataProvider(DataProvider):
    def __init__(self, file_path: str, logger: logging.Logger = None):
        self.file_path = file_path
        self.logger = logger or logging.getLogger(__name__)

    def get_data(self) -> str:
        with open(self.file_path, "r") as f:
            return f.read()

class ApiDataProvider(DataProvider):
    def __init__(self, endpoint_url: str, api_key: str, logger: logging.Logger = None):
        self.endpoint_url = endpoint_url
        self.api_key = api_key
        self.logger = logger or logging.getLogger(__name__)

    def get_data(self) -> str:
        # Code to call the API and get the data.
        pass

class Manager:
    def __init__(self, data_provider: DataProvider, logger: logging.Logger = None):
        self.data_provider = data_provider
        self.logger = logger or logging.getLogger(__name__)

    def __call__(self):
        data = self.data_provider.get_data()
        # Code to process the data.
        pass

if __name__ == "__main__":
    # Example usage:
    logger = logging.getLogger(__name__)
    data_provider = FileDataProvider("path/to/file.txt", logger)
    # data_provider = ApiDataProvider("https://example.com/api", "12345", logger)
    manager = Manager(data_provider, logger)
    manager()
```
In this example, each class has a single, well-defined responsibility, making it easier to understand, maintain, and extend. The DataProvider class is open for extension (new data providers can be added) but closed for modification (the existing code doesn't need to change), and the FileDataProvider and ApiDataProvider classes can be used interchangeably wherever a DataProvider is expected. The Manager class depends on abstractions (DataProvider) rather than concrete implementations (FileDataProvider, ApiDataProvider), making it more flexible and easier to test.
