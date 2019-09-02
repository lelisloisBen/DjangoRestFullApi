# RestFull API with Python and Django
## We are using here the default database, sqlite3

```
cd folderName
pip3 install pipenv
pipenv install django djangorestframework django-rest-knox
django-admin startproject leadmanager
cd leadmanager
python manage.py startapp leads
```

### Inside the folder
-> go to leadmanager folder and setting.py, INSTALLED_APPS = (add the folder 'leads' and 'rest_framework')
-> go to leads folder, models.py and add the table of the database that you want use

### Do the migration of the Model(database file)
```
python manage.py makemigrations leads
python manage.py migrate
```

### Create the serializer (to output info in json)
-> Go to leads folder and create file named 'serializers.py' and insert this lines
```
rom rest_framework import serializers
from leads.models import Lead

# Lead Serializer
class LeadSerializer(serializers.ModelSerializer):
    class Meta:
        model = Lead
        fields = '__all__'
```

### Create API now
-> Inside the leads folder create file named 'api.py' and insert this lines
```
from leads.models import Lead
from rest_framework import viewsets, permissions
from .serializers import LeadSerializer

# Lead Viewset
class LeadViewSet(viewsets.ModelViewSet):
    queryset = Lead.objects.all()
    permission_classes = [
        permissions.AllowAny
    ]
    serializer_class = LeadSerializer
```
### Now add ours URLs 
-> Go to leadmaneger folder and select 'urls.py' add urls
```
path('', include('leads.urls')),
```
### Now create leads urls
-> Go to the leads folder and create a file name it 'urls' insert this lines
```
from rest_framework import routers
from .api import LeadViewSet

router = routers.DefaultRouter()
router.register('api/leads', LeadViewSet, 'leads')

urlpatterns = router.urls
```

### Now start the server and try our RestFull Api (termnal)
```
python manage.py runserver
```
Starting development server at [http://127.0.0.1:8000/]

### Try our RestFull Api with Postman
-> in Postman GET request at the address http://127.0.0.1:8000/api/leads/ (will be empty for now)
-> POST --> http://127.0.0.1:8000/api/leads/ --> headers: key = Content-Type, value: application/json --> body: raw: (has to be json format)
```
{
	"name": "John Doe",
	"email": "jdoe@gmail.com",
	"message": "Please contact John Doe for any informations"
}
```

### Start the server from existing folder
```
cd Desktop/DjangoRestApi
python3 -m venv env
source env/bin/activate
cd leadmanager
python manage.py runserver
```

### To Create super user for python admin
```
cd Desktop/DjangoRestApi
python3 -m venv env
source env/bin/activate
cd leadmanager
python manage.py createsuperuser --email samirbenzada@example.com --username samirbenzada
```
then enter a passord (tou...32)