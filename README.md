# data-table-view

data-table-view is a Django-based web application that simplifies the creation and management of customizable tables, including user registration, login, and data export functionality.

## Table of Contents

1. [Introduction](#introduction)
2. [Features](#features)
3. [Installation](#installation)
4. [Usage](#usage)
5. [Configuration](#configuration)
6. [Examples](#examples)

## Introduction

Welcome to data-table-view, a versatile Django web application designed to streamline the process of creating, managing, and customizing tables for various purposes. Whether you're handling data visualization, project management, or any other scenario that involves structured tabular data, Table Manager simplifies the process. In addition to its robust table management capabilities, this project includes user registration, login, and data export features to provide a comprehensive solution for your data-driven needs. Explore the possibilities and empower your Django projects with Table Manager.

## Features

Table Manager offers a comprehensive set of features to help you effectively manage and customize tables within your Django project:

- Table Management: Easily create, customize, and manage tables, making it a versatile tool for data visualization, reporting, and more.

- User Registration and Login: Implement user registration and login functionality to ensure secure access to table management features.

- Password Reset: Enable users to reset their passwords if they forget them, enhancing user convenience and security.

- Data Export: Utilize the ExportReserves class to export table data in CSV format, making it easy to share and analyze data.

- Customizable Tables: The TableView template allows for flexible customization of tables. Users can define the model, queryset, template, translation for column labels, and even specify the output file name.

- Numerical Field Configuration: Customize the display of numerical fields with the get_numerical_fields method, tailoring the presentation of numeric data to your needs.

- Join Multiple Models: Seamlessly join multiple models together by using the DataTableView and specifying joined columns from other tables, complete with source and label definitions.

- Pagination Control: Have control over pagination settings within tables, ensuring efficient navigation of large datasets.

- Reusability: Take advantage of pre-coded template files (e.g., store/index.html, datatableview/template) that are reusable and require minimal modification to suit your project's styling and layout.

- Email Activation: Implement user account activation via email. Users receive activation links in their email, enhancing the security of your application.

Table Manager empowers you to create dynamic and customizable tables while providing essential user management and data export capabilities, making it a valuable addition to your Django projects.

## Installation

To get started with Table Manager, follow these steps:

1. Database Configuration: First, navigate to the settings.py file in your Django project and edit the DATABASES section. Configure your database connection by specifying your username, password, and the desired database name. Alternatively, you can use SQLite as your database.
   
```python
    DATABASES = {
        'default': {
            'ENGINE': 'django.db.backends.sqlite3',
            'NAME': os.path.join(BASE_DIR, 'db.sqlite3'),
        },
        # Add other database configurations if needed
    }
```

2. Database Setup: Create the database specified in your settings. If you're using SQLite, Django will automatically create the database file for you.

3. Django Installation: If you haven't already installed Django, you can do so using pipenv. Run the following commands within your project directory:


```bash
   pipenv install django
   ...
   pipenv shell
```

 
4. Interpreter Configuration: Ensure that your IDE or code editor is configured to use the Python interpreter provided by pipenv.

5. Database Migration: Apply initial migrations to set up the database schema:


```bash
   python manage.py migrate
```  


6. Run the Development Server: Start the Django development server:

   
```bash
    python manage.py runserver
```
    

Your Table Manager Django project is now up and running. You can access it by navigating to http://localhost:8000/ in your web browser. Explore the user registration, login, and table management features to begin using Table Manager in your project.

Understood, you'd like the "Usage" section to provide a practical code example similar to the TableView part. Here's a revised "Usage" section with an example of how to use TableView to create a table in your Django project:

## Usage

To create a table using TableView, follow these steps by coding it like the example below:

1. Import the Required Classes: Import the TableView class and any other necessary modules in your Django views:

```python
   from django.contrib.auth.decorators import login_required
   from django.utils.decorators import method_decorator
   #import your model
   from .tableview import TableView
   from datatableview import Datatable, columns
```

2. Create a TableView Class: Define a new class for your table view, inheriting from TableView. Customize the class by specifying the model, queryset, template name, translation (column labels), and output file name:

```python
@method_decorator(login_required(login_url="/login"), name="dispatch")
class ProductView(TableView):
    model = YourModel
    queryset = YourModel.objects.select_related('RelatedModel').order_by('id')
    template_name = 'store/index.html'
    translation = {'label': 'field_name'}
    filename = 'export_products.csv'

``` 

3. Customize Numerical Fields: If needed, you can customize the display of numerical fields by overriding the get_numerical_fields method within your CustomTableView class:

   
```python
    def get_numerical_fields(self):
        # Customize numerical fields here
```
    
4.pass context:

```python
     def get_context_data(self, **kwargs):
        context = super().get_context_data(**kwargs)
        context["table_name"] = 'YourTableName'
        return context
```

5. Pagination Configuration: You can specify pagination options within your CustomTableView class by setting the paginate_by attribute:

```python
    class datatable_class(Datatable):
        class Meta:
            page_length = 5
```

Now, you can use the CustomTableView class in your Django views to create and display your customized table. This approach allows you to code your table similar to the TableView example, with the flexibility to customize it further to meet your project's specific requirements.

## Configuration

To configure and customize data-table-view for your Django project, follow these steps:

1. Table Configuration:
   - Configure your tables using the TableView class by inheriting from it, as shown in the "Usage" section above.
   - Define the model, queryset, template name, translation (column labels), and output file name.

2. Numerical Fields Customization:
   - If you need to customize the display of numerical fields, override the get_numerical_fields method within your TableView class.
     
```python
   def get_numerical_fields(self):
       # Customize numerical fields here

```

3.context
  -pass the context data , as shown in the "Usage" section above.

4. Pagination Control:
   - Set the pagination options by adjusting the paginate_by attribute within your TableView class:

  
```python
   page_length = 10  # Modify the number of items per page as needed
```   

5. Database Configuration:
   - In your project's settings.py, configure the database connection in the DATABASES section. You can use the provided SQLite configuration or specify your database settings as required.

  
```python
   DATABASES = {
       'default': {
           'ENGINE': 'django.db.backends.sqlite3',
           'NAME': os.path.join(BASE_DIR, 'db.sqlite3'),
       },
       # Add other database configurations if needed
   }
```

6. User Authentication and Email Activation:
   - Ensure that user registration, login, and password reset functionality are set up according to your project's needs.
   - Configure email settings, including specifying the user and password of the email account used to send activation emails to users. Ensure that users can activate their accounts by clicking on the provided activation URL.

By following these configuration steps, you can tailor data-table-view to match your project's specific requirements and make it a valuable addition to your Django-based data management and visualization toolkit.
## Examples

```python
  from django.contrib.auth.decorators import login_required
from django.utils.decorators import method_decorator
from .models import Product
from .tableview import TableView
from datatableview import Datatable, columns

# Create your views here.

@method_decorator(login_required(login_url="/login"), name="dispatch")
class ProductView(TableView):
    model = Product
    queryset = Product.objects.select_related('collection').order_by('id')
    template_name = 'store/index.html'
    translation = {
        'کد محصول': 'id',
        'نام': 'title',
        'قیمت': 'unit_price',
        'کالکشن': 'collection__title',
        'توضیحات': 'collection__description'
    }
    filename = 'export_products.csv'

    def get_numerical_fields(self):
        return ["unit_price"]

    def get_context_data(self, **kwargs):
        context = super().get_context_data(**kwargs)
        context["table_name"] = 'products'
        return context

    class datatable_class(Datatable):
        collection_title = columns.TextColumn('کالکشن', sources=['collection__title'])
        collection_description = columns.TextColumn('توضیحات', sources=['collection__description'])

        class Meta:
            model = Product
            ordering = ["id"]
            page_length = 5
            columns = ["id", "title", "unit_price", "collection_title", "collection_description"]
            labels = {
                "id": 'کد محصول',
                "title": "نام",
                "unit_price": 'قیمت',
            }              

```
