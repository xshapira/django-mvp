This is an opinionated Django cookie cutter template. I use this template for quick MVPs and prototypes that might be expected to grow over time, so solid foundations based on tried-and-tested solutions are important.

For a more configurable and complex cookiecutter, see https://github.com/pydanny/cookiecutter-django.

It is assumed that you have basic familiarity with Django, Heroku and AWS.

## Features

* Heroku, AWS/S3 and Mailgun in production
* Docker for local development
* Turbolinks/StimulusJS for JS
* Tailwind/PostCSS for CSS
* Celery
* Redis
* PostgreSQL

## Getting Started

You should have the following installed on your development machine:

* Docker and docker-compose
* Cookiecutter (pip install cookiecutter)
* pre-commit hooks (pip install pre-commit)
* prettier (npm install -g prettier)
* eslint (npm install -g eslint)

Now build the project:

    cookiecutter https://github.com/danjac/cookiecutter-django-turbolinks-tailwind

Change to your project directory and copy the file .env.example to .env. Set the **SECRET_KEY**. You can change the AWS settings when you need to deploy to production.

Build your docker environment:

    docker-compose build

A custom user model is provided. Open *users/models.py* and add any custom fields you might want at the start.

Next run migrations:

    ./scripts/manage makemigrations users

    ./scripts/manage migrate

You can run any other Django management commands locally with *./scripts/manage*.

To run unit tests with pytest:

    ./scripts/runtests  [ARGS]

You can now start the application:

    docker-compose up -d

To ensure all dependencies are up to date, you can use pip-tools (pip install piptools):

    pip-compile requirements.in -o requirements.txt

And for frontend dependencies:

    ./scripts/yarn update


## Templates

Django-allauth is used for authentication. This package has its own set of templates that should be overridden if you want to have Tailwind-styled login and sign up pages. You can do this by copying these templates into an *account/* folder under the *templates* folder in your project:

https://github.com/pennersr/django-allauth/tree/master/allauth/templates

## Tailwind

You can add custom classes in the file *static/css/app.css* using the *@apply* directive. Compilation should be on-the-fly using PostCSS. See the Tailwind docs for more details.

## Stimulusjs

Turbolinks and Stimulusjs have been integrated to provide full frontend interactivity without the complexity and overhead of an SPA architecture. A few simple controllers are included to get started with.

## Deployment

Ensure you have the correct AWS settings in your .env file. BASE_URL should point to your CDN.

PostgreSQL and Redis buildpacks are required.

Set up Heroku and Git as per the instructions. You need to set up Heroku to deploy a Docker image:

https://devcenter.heroku.com/articles/git

https://www.heroku.com/deploy-with-docker

To deploy just run:

    ./scripts/deploy

This is a very barebones deployment that just pushes assets to S3 and deploys your application to Heroku. You may wish to expand this to use a CI/CD pipeline such as Github Actions or Travis, or Ansible or Terraform for provisioning servers, manage staging environments etc.

## References

* Turbolinks: https://github.com/turbolinks/turbolinks
* StimulusJS: https://stimulusjs.org/
* Tailwind: https://tailwindcss.com/
