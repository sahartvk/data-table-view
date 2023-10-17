# data-table-view-fa

data-table-view is a Django-based web application that simplifies the creation and management of customizable tables, including user registration, login, and data export functionality.

## Table of Contents

1. [Introduction](#introduction)
2. [Features](#features)
3. [Installation](#installation)
4. [Usage](#usage)
5. [Examples](#examples)

## Introduction

Welcome to data-table-view, a versatile Django web application designed to streamline the process of creating, managing, and customizing tables for various purposes. Whether you're handling data visualization, project management, or any other scenario that involves structured tabular data, Table Manager simplifies the process. Explore the possibilities and empower your Django projects with Table Manager.

## Features

data-table-view-fa offers a comprehensive set of features to help you effectively manage and customize tables within your Django project:

- Table Management: Easily create, customize, and manage tables, making it a versatile tool for data visualization, reporting, and more.

- Data Export: Utilize the ExportReserves class to export table data in CSV format, making it easy to share and analyze data.

- Customizable Tables: The TableView template allows for flexible customization of tables. Users can define the model, queryset, template, translation for column labels, and even specify the output file name.

- Pagination Control: Have control over pagination settings within tables, ensuring efficient navigation of large datasets.

- Filter by Column: One of the key features of DataTable View is the ability to filter data based on each column. A filter menu is displayed, When you click the "Apply Filter" button, the filter is applied to the table, indicating that filtering is active. 

-Additionally, the search box for the entire table and specifying the number of entries per page are also included.
However, if you wish to customize the styling, please refer to static/css/styles.css and comment the following lines to modify or hide them:

```css
/* comment and modify as needed
.dataTables_filter {
    /* Your custom styling here */
}

.dataTables_length {
    /* Your custom styling here */
}
*/
```

data-table-view-fa empowers you to create dynamic and customizable tables while providing essential user management and data export capabilities, making it a valuable addition to your Django projects.

## Installation

To get started with this project, follow these steps:

### 1. Install django-datatable-view

You need to install the `django-datatable-view` package, which is used for creating data tables in Django. You can install it using pip:

```bash
pip install django-datatable-view
```


### 2. Add DataTable View to Installed Apps

In your Django project's settings (`settings.py`), add `'datatableview'` to the `INSTALLED_APPS` list:

```python
# settings.py
INSTALLED_APPS = [
    'datatableview',
    # ...
]
```


### 3. add Static Files

Make sure to configure Django to serve static files. Add static files to your project:

### 4. Add .py Files and Templates to Your Project

You will need to include specific Python files and templates from this project into your Django project. Follow these steps to integrate them:

#### .py Files

Copy the required `.py` files from this project to your Django app. Ensure they are in the right directory structure.

#### Templates

Include the templates in your Django app's template directory, keeping the same directory structure as in this project.


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
    template_name = 'datatableview/index.html'
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

6.structure

```bash

└── YourProject
    ├── static
    │   ├── css
    |   ├── DataTables
    |   ├── fonts
    │   └── js
    └── YourApp
        └── templates/datatableview
        |    ├── index.html
        |    └── template.html
        ├── ExportReserves.py
        ├── tableview.py
        ├── ...
        └── views.py

```


## Examples

views.py:

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
    template_name = 'datatableview/index.html'
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
