## Consultas XQuery
### <ins>**Consultas "libros.xml"**</ins>
1-Títulos ordenados.

Query:
```
for $x in doc("libros.xml")/libros/libro
order by $x/titulo
return $x/titulo
```
Result:

```
<titulo>El Quijote</titulo>
<titulo>Programas ERP</titulo>
```

2-Títulos ordenados por el nombre del primer autor.

Query:
```
for $x in doc("libros.xml")/libros/libro
order by $x/auto[1]
return $x/titulo
```
Result:

```
<titulo>El Quijote</titulo>
<titulo>Programas ERP</titulo>
```

3-Nombre y apellido del primer autor de cada libro.

Query:
```
for $x in doc("libros.xml")/libros/libro/autores
return $x/autor[1]
```
Return:
```
<autor>
                <nombre>Miguel</nombre>
                <apellido>Cervantes</apellido>
            </autor>
<autor>
                <nombre>Axel</nombre>
                <apellido>García</apellido>
                <email>Axel@abc.com</email>
            </autor>
```

4-Título y número de autores de cada libro.

Query:
```
for $x in doc("libros.xml")/libros/libro
let $count := count($x/autores/autor)
return
<resultado>
 <titulo>{data($x/titulo)}</titulo>
  <autores>{$count}</autores>
</resultado>
```
Result:
```
<resultado>
  <titulo>El Quijote</titulo>
  <autores>1</autores>
</resultado>
<resultado>
  <titulo>Programas ERP</titulo>
  <autores>2</autores>
</resultado>
```

5-Libros que tienen varios autores y libros que tienen un autor.

Libros con más de 1 autor
Query:
```
for $x in doc("libros.xml")/libros/libro
let $count := count($x/autores/autor)
where $count >1
return <titulo>{data($x/titulo)}</titulo>
```
Result:
```
<titulo>Programas ERP</titulo>
```
Libros con solo 1 autor:

Query:
```
for $x in doc("libros.xml")/libros/libro
let $count := count($x/autores/autor)
where $count =1
return <titulo>{data($x/titulo)}</titulo>
```
Result:
```
<titulo>El Quijote</titulo>
```

6-Libros cuyo autor es “Axel”.

Query:
```
for $x in doc("libros.xml")/libros/libro
where $x/autores/autor/nombre ='Axel'
return <titulo>{data($x/titulo)}</titulo>
```
Result:
```
<titulo>Programas ERP</titulo>
```

7-Mostrar título y precio en una lista HTML.

```
<ul>
{
  for $x in doc("libros.xml")/libros/libro
  return
  <libro>
  <titulo>{data($x/titulo)}</titulo>
  <precio>{data($x/precio)}</precio>
  </libro>
}
</ul>
```

8-Mostrar: título, isbn y precio en una tabla HTML.

```
<ul>
{
  for $x in doc("libros.xml")/libros/libro
  return
  <libro>
  <titulo>{data($x/titulo)}</titulo>
  <ref>{data($x/isbn)}</ref>
  <precio>{data($x/precio)}</precio>
  </libro>
}
</ul>
```

### <ins>**Consultas "alumnos.xml"**</ins>

1- Mostrar el nombre de los alumnos aprobados.

Query:
```
for $x in doc("alumnos.xml")/alumnos/alumno
where $x/nota >4
return
<nombre>{data($x/nombre)}</nombre>
```
Result:

```
<nombre>Jose</nombre>
<nombre>Sonia</nombre>
<nombre>Ana</nombre>
```

2- Mostrar el DNI y la nota de los alumnos que han aprobado.

Query:
```
for $x in doc("alumnos.xml")/alumnos/alumno,
$attr in $x/@dni
where $x/nota >4
return
<resultado>
    <dni>{data($attr)}</dni>
    <nota>{data($x/nota)}</nota>
</resultado>
```
Result:
```
<resultado>
  <dni>41293940</dni>
  <nota>7</nota>
</resultado>
<resultado>
  <dni>23456782</dni>
  <nota>8</nota>
</resultado>
<resultado>
  <dni>12345678</dni>
  <nota>9</nota>
</resultado>
```


3- Indicar el nombre de los alumnos cuyas notas están entre 6 y 8 (ambas inclusive).

Query:
```
for $x in doc("alumnos.xml")/alumnos/alumno
where $x/nota >=6 and $x/nota <=8
return
<nombre>{data($x/nombre)}</nombre>
```
Result:
```
<nombre>Jose</nombre>
<nombre>Sonia</nombre>
```
4- Mostrar listado de nombres ordenados por apellidos.

Query:
```
for $x at $i in doc("alumnos.xml")/alumnos/alumno
order by $x/apells
return
<nombre>{$i}-{data($x/nombre)}</nombre>
```
Result:
```
<nombre>1-Jose</nombre>
<nombre>3-Juan</nombre>
<nombre>2-Sonia</nombre>
<nombre>5-Ana</nombre>
<nombre>4-Manuel</nombre>
```
5- Mostrar nombres ordenados por DNI.

Query:
```
for $x in doc("alumnos.xml")/alumnos/alumno,
$attr in $x/@dni
order by $attr
return
<nombre>{data($attr)}-{data($x/nombre)}</nombre>
```
Result:
```
<nombre>12345678-Ana</nombre>
<nombre>12367845-Juan</nombre>
<nombre>23456782-Sonia</nombre>
<nombre>34567821-Manuel</nombre>
<nombre>41293940-Jose</nombre>
```

6- Mostrar el Nº de cada alumno y su nombre.

Query:
```
for $x at $i in doc("alumnos.xml")/alumnos/alumno
return
<nombre>{$i}-{data($x/nombre)}</nombre>
```
Result:
```
<nombre>1-Jose</nombre>
<nombre>2-Sonia</nombre>
<nombre>3-Juan</nombre>
<nombre>4-Manuel</nombre>
<nombre>5-Ana</nombre>
```
