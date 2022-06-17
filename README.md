## Work process

Create a copy of this repository and complete the task in your repository. 
When the task is complete add these accounts as collaborators: `armen9494`, `cripalani` 

## Installation

### Backend

Install all the dependencies using composer

```
composer install
```

Copy the example env file and make the required configuration changes in the .env file

```
cp .env.example .env
```

Generate a new application key

```
php artisan key:generate
```

Set the database connection in `.env`.

Run the database migrations with seeds.

```
php artisan migrate:fresh --seed
```

### Frontend

Install all packages using npm

```
npm install
```

Run

```
npm run dev
```

## Task description
