# rcc-pm-simulacion-client-php

Esta API simula el Reporte de Crédito Consolidado de Persona Moral.

## Requisitos

PHP 7.1 ó superior

### Dependencias adicionales

- Se debe contar con las siguientes dependencias de PHP:
  - ext-curl
  - ext-mbstring
- En caso de no ser así, para linux use los siguientes comandos

```sh
#ejemplo con php en versión 7.3 para otra versión colocar php{version}-curl
apt-get install php7.3-curl
apt-get install php7.3-mbstring
```

- Composer [vea como instalar][1]

## Instalación

Ejecutar: `composer install`

## Guía de inicio

### Paso 1. Agregar el producto a la aplicación

Al iniciar sesión seguir los siguientes pasos:

1.  Dar clic en la sección "**Mis aplicaciones**".
2.  Seleccionar la aplicación.
3.  Ir a la pestaña de "**Editar '@tuApp**' ".
    <p align="center">
      <img src="https://github.com/APIHub-CdC/imagenes-cdc/blob/master/edit_applications.jpg" width="900">
    </p>
4.  Al abrirse la ventana emergente, seleccionar el producto.
5.  Dar clic en el botón "**Guardar App**":
    <p align="center">
      <img src="https://github.com/APIHub-CdC/imagenes-cdc/blob/master/selected_product.jpg" width="400">
    </p>

### Paso 2. Capturar los datos de la petición

Los siguientes datos a modificar se encuentran en **test/Api/ApiTest.php**

Es importante contar con el setUp() que se encargará de inicializar la petición. Por tanto, se debe modificar la URL (**url_API**); y la API KEY (**x_api_key**), como se muestra en el siguiente fragmento de código:

```php
public function setUp()
{
    $config = new Configuration();
    $config->setHost('the_url');
    $this->x_api_key = "your_x_api_key";
    $client = new Client();
    $this->apiInstance = new Instance($client,$config);
}
```

Para la petición se deberá modificar el siguiente fragmento de código con los datos correspondientes:

> **NOTA:** Para más ejemplos de simulación, consulte la colección de Postman.

```php
/**
* Este método será ejecutado en la prueba ubicado en path/to/repository/test/Api/ApiTest.php
*/
public function testGetReporteCreditoPM()
{
    try {

        $request = new PersonaPeticion();
        $persona = new Persona();
        $domicilio = new PersonaDomicilio(); 
        $catalogoEstados = new CatalogoEstados(); 
        $catalogoPais = new CatalogoPais();            

        $domicilio->setDireccion("AV. PASEO DE LA REFORMA 01");
        $domicilio->setColoniaPoblacion("GUERRERO");
        $domicilio->setDelegacionMunicipio("CUAUHTEMOC");
        $domicilio->setCiudad("CIUDAD DE MÉXICO");
        $domicilio->setEstado($catalogoEstados::DF);
        $domicilio->setCP("68370");
        $domicilio->setPais($catalogoPais::MX);

        $persona->setRFC("EDC930121E01");
        $persona->setNombre("RESTAURANTE SA DE CV");
        $persona->setDomicilio($domicilio);

        $request->setFolioOtorgante("1000001");
        $request->setPersona($persona);

        $result = $this->apiInstance->getReporteCreditoPM($this->x_api_key, $request);
        print_r($result);
    } catch (ApiException | Exception $e) {
        echo 'Exception when calling ApiTest->testGetReporteCreditoPM: ', $e->getMessage(), PHP_EOL;
    }        
}
```

## Pruebas unitarias

Para ejecutar las pruebas unitarias:

```sh
./vendor/bin/phpunit
```

---
[CONDICIONES DE USO, REPRODUCCIÓN Y DISTRIBUCIÓN](https://github.com/APIHub-CdC/licencias-cdc)

[1]: https://getcomposer.org/doc/00-intro.md#installation-linux-unix-macos
