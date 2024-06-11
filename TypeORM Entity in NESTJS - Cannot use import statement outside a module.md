# TypeORM Entity en NESTJS - No se puede usar la declaración de importación fuera de un módulo

**Pregunta hecha hace:** 4 años, 5 meses  
**Modificada hace:** 10 días  
**Vistas:** 149k veces  
**Votos:** 139

Empecé un nuevo proyecto con el comando `nest new`. Funciona bien hasta que agrego un archivo de entidad.

Obtuve el siguiente error:

```javascript
import { Entity, Column, PrimaryGeneratedColumn } from 'typeorm';

^^^^^^

SyntaxError: Cannot use import statement outside a module
```

## ¿Qué me estoy perdiendo?

### Agregando la Entidad al Módulo:

```javascript
import { Module } from '@nestjs/common';
import { BooksController } from './books.controller';
import { BooksService } from './books.service';
import { BookEntity } from './book.entity';
import { TypeOrmModule } from '@nestjs/typeorm';

@Module({
  imports: [TypeOrmModule.forFeature([BookEntity])],
  controllers: [BooksController],
  providers: [BooksService],
})
export class BooksModule {}
```

### app.module.ts:

```javascript
import { Module } from '@nestjs/common';
import { AppController } from './app.controller';
import { AppService } from './app.service';
import { TypeOrmModule } from '@nestjs/typeorm';
import { Connection } from 'typeorm';
import { BooksModule } from './books/books.module';

@Module({
  imports: [TypeOrmModule.forRoot()],
  controllers: [AppController],
  providers: [AppService],
})
export class AppModule {}
```

## Solución

Mi suposición es que tienes una configuración de TypeormModule con una propiedad `entities` que se ve así:

```javascript
entities: ['src/**/*.entity.{ts,js}']
```

o así:

```javascript
entities: ['../**/*.entity.{ts,js}']
```

El error que estás obteniendo es porque estás intentando importar un archivo `.ts` en un contexto `.js`. Mientras no estés usando webpack, puedes usar esto en su lugar para obtener los archivos correctos:

```javascript
entities: [join(__dirname, '**', '*.entity.{ts,js}')]
```

donde `join` es importado del módulo `path`. Ahora `__dirname` se resolverá a `src` o `dist` y luego encontrará el archivo `.ts` o `.js` esperado respectivamente. Déjame saber si todavía hay algún problema.
