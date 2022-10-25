### Ejemplos vistos | 4CSTC | Bases de Datos    | MySQL - PHP

##### Funciones 

```php
function conectar_con_mysql($servername,$username,$password){ 
    
    $conexion = mysqli_connect($servername, $username, $password);
    if ($conexion) {
        echo "<h3> Conexión con MySQL exitosa </h3>";
    } else {
        echo "<p> Error de conexión: " . mysqli_error() . "</p>";
    }
    return $conexion;
}

function conectar_con_base_de_datos($nombre_db,$servername,$username,$password){
    
    $conexion = mysqli_connect($servername, $username, $password, $nombre_db);
    if ($conexion) {
        echo "<h3> Conexión con base de datos exitosa. </h3>";
    } else {
        echo "<p> Error de conexión: " . mysqli_error() . "</p>";
    }
    return $conexion;
}

/*
 * La siguiente función sirve para:
     * Creación de bases de datos
     * Creación de tablas
     * inserción simple
     * actualización de datos
     * eliminación de datos
 */

function ejecutar_consulta($nombre_db,$conexion,$consulta_sql) {
    
    if (mysqli_query($conexion, $consulta_sql)) {
        echo "<p> Ejecución exitosa de consulta " . $consulta_sql . "</p>";
    } else {
        echo "<p> Error " . mysqli_error($conexion) . "</p>";
    }
}

function insercion_multiple($conexion,$nombre_de_tabla,$consulta_sql){

    if (mysqli_multi_query($conexion, $consulta_sql)) {
        echo "<p> inserciones realizadas con éxito </p>";
    } else {
        echo "<p> Error: " . mysqli_error($conexion) . '</p>';
    }
}

function seleccionar_todo($conexion,$nombre_de_tabla,$consulta_sql){
    
    $result = mysqli_query($conexion, $consulta_sql);

    if (mysqli_num_rows($result) > 0) {

        $respuesta = '<h4> Datos obtenidos de la tabla "' . $nombre_de_tabla . '"</h4>';

        for ($i = 0; $i < mysqli_num_rows($result); $i++) {
            $fila = mysqli_fetch_assoc($result);
            $respuesta .= "<p> id: " . $fila["id"] . " - Nombre completo: " .
                               $fila["nombre"] . ' '. $fila["apellido"]. "</p>";
        }

        echo $respuesta;

    } else {
        echo "<p> No se han encontrado resultados. </p>";
    }
}


function conectar_y_obtener_datos($nombre_de_base_de_datos,$nombre_tabla) {
    $conexion = conectar_a_base_de_datos($nombre_de_base_de_datos);
    seleccionar_todo($conexion,$nombre_tabla);
}


// función para los ejemplos:
function conectar_y_obtener_datos() {
    $conexion = conectar_a_base_de_datos('ejemplos');
    seleccionar_todo($conexion,'personas');
}
```

##### Ejemplo 1:

```php
<!DOCTYPE html>
<html lang="en">

    <head>
        <meta charset="UTF-8">
        <title> PHP - MySQL</title>
        <?php include 'funciones.php'; ?>
    </head>

    <body>

        <ul>
            <?php conectar_y_obtener_datos(); ?>
        </ul>

    </body>
</html>
```

##### Ejemplo 2:

```php
<!DOCTYPE html>
<html lang="en">
    
    <head>
        <meta charset="UTF-8">
        <title> PHP - MySQL </title>
        <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.6.1/jquery.min.js"></script>
        <?php include 'funciones.php'; ?>
    </head>
    
    <body>
        
        <button onclick="obtenerDatosConPHP()"> 
            OBTENER DATOS CON PHP
        </button>
        
        <div id="contenido">
        
        </div>
        
        <script>

            function obtenerDatosConPHP() {
                let resultado = "<?php   conectar_y_obtener_datos();   ?>";
                $('#contenido').html(resultado);
            }
        
        </script>
        
    </body>
</html>
```

##### Ejemplo 3:

```php
<!DOCTYPE html>
<html lang="en">
    
    <head>
        <meta charset="UTF-8">
        <title> PHP - MySQL </title>
        <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.6.1/jquery.min.js"></script>
        <?php include 'funciones.php'; ?>
    </head>
    
    <body>
        
        <button onclick="obtenerDatosConAJAX()"> 
            OBTENER DATOS CON AJAX 
        </button>
        
        <div id="contenido">
        
        </div>
        
        <script>

            function obtenerDatosConAJAX() {
                $.ajax({
                    type: "GET",
                    url: "mostrarDatosMySQL_2.php"
                }).done(function (respuesta) {
                    $('#contenido').html(respuesta);
                });
            }
        
        </script>
        
    </body>
</html>
```
```php
<?php 

    // mostrarDatosMySQL_2.php
    
    include 'funciones.php';
    conectar_y_obtener_datos();

?>
```

##### Ejemplo 4:

```php
<!DOCTYPE html>
<html lang="en">
    
    <head>
        <meta charset="UTF-8">
        <title> PHP - MySQL </title>
        <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.6.1/jquery.min.js"></script>
        <?php include 'funciones.php'; ?>
    </head>
    
    <body>
        
        <button onclick="enviarInEjemplocionConAJAX()"> 
            ENVIAR DATOS CON AJAX
        </button>
        
        <div id="contenido">
        
        </div>
        
        <script>

            function enviarInEjemplocionConAJAX() {
    
                let nombre = 'Gloria';
                let listaDeNumeros = [1, 3, 4, 5];
                let inEjemplocion = {
                                    nombre_admin : nombre,
                                    ids_usuarios : listaDeNumeros
                                  }
    
                $.ajax({
                    type: "POST",
                    url: "respuesta4.php",
                    data: inEjemplocion
                }).done(function (respuesta) {
                    $('#contenido').html(respuesta);
    
                });
            }
        
        </script>
        
    </body>
</html>
```
```php
<?php

    // respuesta4.php
    
    echo '<h3> Datos recibidos mediante la petición POST: </h3>';

    if (isset($_POST['nombre_admin']) and isset($_POST['ids_usuarios'])) {

        echo '<p>' . $_POST['nombre_admin']. '</p>';

        for ($x = 0; $x < count($_POST['ids_usuarios']); $x++) {
           echo '<p>' . $_POST['ids_usuarios'][$x] . '</p>';
        }
    }

?>
```


##### Ejemplo 5:

```php
<!DOCTYPE html>
<html lang="en">

    <head>
        <meta charset="UTF-8">
        <title>Ejemplo </title>
    </head>
    
    <body>

        <form action="/respuesta2.php" method="post">
            <div class="form-group">
                <label for="nombre"> Nombre </label>
                <input type="text" name="nombre" placeholder="Enter name">
            </div>
            <div class="form-group">
                <label for="email">Email </label>
                <input type="email" name="email" placeholder="Enter email">
            </div>
            
            <button type="submit"> Submit </button>
        </form>
        
    </body>

</html>
```
```php
<!DOCTYPE html>
<html lang="en">

    <head>
        <meta charset="UTF-8">
        <title> Ejemplos de visualizar datos </title>
    </head>

    <body>

        <div>
            <p> Bienvenido, <?php echo $_POST["nombre"] ?> </p>
            <?php echo '<p> Su email es: ' . $_POST["email"] . '</p>' ?>
        </div>

        <br>

        <?php
            
            // respuesta2.php

            $contenido = '<div>' .
                             '<p> Bienvenido, ' . $_POST["nombre"] . '</p>' .
                             '<p> Su email es: ' . $_POST["email"] . '</p>' .
                         '</div>';
            echo $contenido;

        ?>
    </body>
</html>
```

##### Ejemplo 6:
```php
<!DOCTYPE html>
<html lang="en">

    <head>
        <meta charset="UTF-8">
        <title> Transferencia de datos </title>
    </head>

    <body>

        <a href='respuesta3.php?name=Esteban'> MOSTRAR NOMBRE </a>
        <a href='respuesta3.php?cantidad=123'> MOSTRAR CANTIDAD </a>
        <a href='respuesta3.php?cantidad=123&name=Esteban'> MOSTRAR TODOS LOS DATOS </a>

        <br>

        <?php

            $contenido = '<div class="border border-success rounded">' .
                             '<p> Bienvenido, ' . $_POST["nombre"] . '</p>' .
                             '<p> Su email es: ' . $_POST["email"] . '</p>' .
                         '</div>';
            echo $contenido;

        ?>
    </body>
</html>
```
```php
<!DOCTYPE html>
<html lang="en">

    <head>
        <meta charset="UTF-8">
        <title> Transferencia de datos </title>
    </head>

    <body>
    
        <?php
            
            // respuesta3.php
            
            echo '<h3> Datos: </h3>';
            echo '<p>' . $_GET["name"] . '</p>';
            echo '<p>' . $_GET['cantidad'] . '</p>';
            
        ?>

    </body>
</html>
```

##### Sessions

```php
<?php
    
    /*
    Comandos necesarios:
        sudo apt install php-mysqli
        php -S localhost:8000 -t nombreDeLaCarpeta/
    */
   
    
    
    session_start();
    
    $mensaje = '<h4> Número de identificación en sesión actual: </h4>' .
               '<p> id: ' . session_id() . '</p>' ;
    echo $mensaje;
    
    $_SESSION['nombre_de_usuario'] = $nombre_recibido;
    $_SESSION['email'] = $email_recibido;
    
    $valores_persistentes = '<h4> Muestra de valores definidos en la sesión: </h4>' .
                            '<p>' . $_SESSION['nombre_de_usuario'] . '</p>' .
                            '<p>' . $_SESSION['email'] . '</p>';
    
    echo $valores_persistentes;
    
    /*
     
        Forma de destruir la session:
        session_destroy();
 
    */

    //Otras operaciones:
    unset($_SESSION['id_ususario']);
    isset($_SESSION['id_ususario'])
   
?>
```


