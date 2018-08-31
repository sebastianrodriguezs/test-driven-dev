# Daily log 

## This log is for keeping track of my progress and to mark the #100daysOfCode challenge, everyday

***

#### August 30th, 2018

In reality, I had started three days ago, but it was until today that I decided to keep track of things, and in a more serious manner. 

**_Notes_**

1. __Unit tests, and how they differ from Functional Tests__

    > Unit tests test the application from the inside, from the point of view of the programmer.

    + The difference between the two is the point of view, since the Functional tests describe the new functionality from the user's point of view.

2. __Django has an MVC structure (*Model-View-Controller*)__

    > Well, *broadly*. It definitely does have models, but its views are more like a controller, and it's the templates that are actually the view part, but the general idea is there. If you're interested, you can look up the finer points of the discussion [in the Django FAQs.](https://docs.djangoproject.com/en/1.11/faq/general/)

3. __How the unit test is working until now__

    + First we wrote the unit test:
    ```python
    class HomePageTest(TestCase):
        found = resolve('/')
        self.assertEqual(found.func, home_page)
    ```

    + After the expected failure, which was an exception (ImportError), we can now start to code because of a failing Functional Test and a failing Unit test. Now we modified *list/views.py*:
    ```python
    from django.shortcuts import render

    # Create your views here.
    home_page = None
    ```

    + Django tried to resolve (the function that internally resolves URLs and finds what view function they should map to) "/", and raised a 404 error, meaning that it could not find a URL mapping for "/".

    + Next, in *superlists/urls.py*, which is the main *urls.py* of the whole site, we removed the admin url, and added the "/" url, with a regular expression:
    ```python
    from django.conf.urls import url
    from lists import views

    urlpatterns = [
        url(r'^$', views.home_page, name = 'home'),
    ]
    ```

    + When running the test again, we no longer get a 404 error, but instead a TypeError, complaining that ```home_page``` is not callable. And now is the time to write an actual function instead of having ```home_page = None```. 
    > Every single code is driven by the tests!

4. __A new unit test__
    >
        1. First we create an HttpRequest object, which is what Django will see when a user's browser asks for a page
        2. We pass it to our ```home_page``` view, which gives us a response. You won't be surprised to hear that this object is an instance of a class called HttpResponse.
        3. Then, we extract the .content of the response. These are the raw bytes, the ones and zeros that would be sent down the wire to the user's browser. We call .decode() to convert them into the string of HTML that's being sent to the user.
        4. We want it to start with an <html> tag which gets closed at the end.
        5. And we want a <title> tag somewhere in the middle, with the words "To-Do lists" in it —because that's what we specified in our functional test.
    
#### The Unit-Test/Code cycle

> 
    We can start to settle into the TDD unit-test/code cycle now:

    1. In the terminal, run the unit tests and see how they fail.
    2. In the editor, make a minimal code change to address the current test failure.
    
    And repeat!


    
