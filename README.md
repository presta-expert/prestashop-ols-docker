# README

Docker image of OpenLiteSpeed server with Prestashop and ionCube installed _(appropriate for the given php version)._

## Arguments

### Build arguments
You can specify a few helpful arguments when building an image.

| **Argument**         | **Description**                     | **Default** |
|----------------------|-------------------------------------|-------------|
| _OLS_VERSION_        | Version of OpenLiteSpeed to use     | _1.7.16_    |
| _PHP_VERSION_        | Version of PHP to use (without dot) | _74_        |
| _PRESTASHOP_VERSION_ | Version of Prestashop to run        | _1.7.8.8_   |

**Example:**
```bash
docker build -t [target-name] --build-arg OLS_VERSION="1.7.16" --build-arg PHP_VERSION="81" src/
```

### Prestashop PHP compatibility chart

Consider to use **_Prestashop PHP compatibility chart_** to set correct PHP and Prestashop versions:

| **Line** | **Link**                                                                                                    |
|----------|-------------------------------------------------------------------------------------------------------------|
| _1.7_    | https://devdocs.prestashop-project.org/1.7/basics/installation/system-requirements/#php-compatibility-chart |
| _8.0_    | https://devdocs.prestashop-project.org/8/basics/installation/system-requirements/#php-compatibility-chart   |

## Authors

- Presta.Expert Team  <support@presta.expert>

## License

The files in this archive are released under the [MIT LICENSE](LICENSE).