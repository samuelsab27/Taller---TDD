# Taller de TDD - Pruebas Unitarias

## Nombre de Integrantes: Samuel Sabogal Espinel Leon🦁, Julian Andres Vasquez Pedraza

Este proyecto implementa el enfoque **Test-Driven Development (TDD)** aplicado a una **Arquitectura Limpia (Clean Architecture)**.  
El objetivo es desarrollar reglas de negocio en el dominio de forma aislada, garantizando calidad y mantenibilidad mediante pruebas unitarias.

---

## Objetivos

- Diseñar pruebas unitarias que validen reglas de negocio sin depender de frameworks o infraestructura.  
- Aplicar el ciclo **TDD (Red → Green → Refactor)** de forma iterativa.  
- Escribir pruebas con el patrón **AAA (Arrange – Act – Assert)** para mayor claridad.  
- Usar **clases de equivalencia** y **valores límite** para cubrir escenarios válidos, inválidos y extremos.  
- Expresar pruebas con **BDD (Given – When – Then)** para mantener trazabilidad con los requisitos.

---
## Dominio

-El sistema modela el registro de votantes para una registraduría, donde se validan las reglas de negocio antes de permitir el registro.

# Clases principales:

-Person: representa una persona con atributos id, name, age, gender, alive.

-Registry: servicio que valida las reglas para registrar votantes.

-RegisterResult: enum con resultados posibles (VALID, INVALID, DEAD, DUPLICATED, etc).

##  Reglas de negocio validadas

| Regla | Descripción | Resultado esperado |
|-------|--------------|--------------------|
| Persona viva con edad ≥ 18 y única | Válido para votar | `VALID` |
| Persona nula | Entrada inválida | `INVALID` |
| Edad < 0 o > 120 | Edad fuera de rango | `INVALID_AGE` |
| Edad < 18 | No es votante | `UNDERAGE` |
| Persona muerta | No puede registrarse | `DEAD` |
| Persona duplicada (mismo id) | Registro repetido | `DUPLICATED` |

## Ciclo TDD aplicado
Iteración	Estado	Descripción
1	🔴 Rojo	Se crea la prueba shouldRegisterValidPerson() que inicialmente falla.
2	🟢 Verde	Se implementa el método registerVoter() para retornar VALID.
3	🔵 Refactor	Se mejora el código y se agregan más validaciones (DEAD, INVALID, UNDERAGE).
🧪 Patrón AAA en las pruebas

# Ejemplo:

@Test
public void shouldRegisterValidPerson() {
    // Arrange
    Registry registry = new Registry();
    Person person = new Person("Ana", 1, 30, Gender.FEMALE, true);

    // Act
    RegisterResult result = registry.registerVoter(person);

    // Assert
    Assert.assertEquals(RegisterResult.VALID, result);
}

## 🧩 Escenarios BDD (Behavior Driven Development)

| Test | Escenario (Given – When – Then) |
|------|----------------------------------|
| shouldReturnInvalidWhenPersonIsNull | Dado que la persona es null, cuando intento registrarla, entonces el resultado debe ser `INVALID`. |
| shouldRejectWhenIdIsZeroOrNegative | Dado que la persona tiene id inválido (≤ 0), cuando intento registrarla, entonces el resultado debe ser `INVALID`. |
| shouldRejectUnderageAt17 | Dado que la persona tiene 17 años, cuando intento registrarla, entonces el resultado debe ser `UNDERAGE`. |
| shouldAcceptAdultAt18 | Dado que la persona tiene 18 años, cuando intento registrarla, entonces el resultado debe ser `VALID`. |
| shouldRejectInvalidAgeOver120 | Dado que la persona tiene 121 años, cuando intento registrarla, entonces el resultado debe ser `INVALID_AGE`. |

## Matriz de clases de equivalencia


| Caso | Entrada | Resultado esperado | Test asociado |
|------|----------|--------------------|----------------|
| Persona viva, 30 años, id único | (edad=30, vivo=true, id=1) | `VALID` | shouldRegisterValidPerson |
| Persona muerta | (edad=45, vivo=false) | `DEAD` | shouldRejectDeadPerson |
| Edad 17 | (edad=17, vivo=true) | `UNDERAGE` | shouldRejectUnderageAt17 |
| Edad -1 | (edad=-1, vivo=true) | `INVALID_AGE` | shouldRejectInvalidAgeNegative |
| Edad 121 | (edad=121, vivo=true) | `INVALID_AGE` | shouldRejectInvalidAgeOver120 |
| Persona duplicada | (id=777 repetido) | `DUPLICATED` | shouldRejectDuplicatePerson |

## Gestión de defectos (defectos.md)

-Ejemplo:

### Defecto 01
- Caso: edad -1
- Esperado: INVALID_AGE
- Obtenido: VALID
- Causa probable: falta de validación de límites
- Estado: Abierto


## Conclusiones

El enfoque TDD permite construir código guiado por reglas de negocio desde el inicio.

El patrón AAA mejora la legibilidad y mantenibilidad de las pruebas.

Las clases de equivalencia y valores límite garantizan una cobertura representativa con menos pruebas.

BDD facilita la trazabilidad entre los requisitos del negocio y las pruebas automatizadas.
## Recursos recomendados

# The Art of Software Testing – Myers (2011)

# Testing Computer Software – Kaner (1999)

# Effective Unit Testing – Lasse Koskela (2013)
