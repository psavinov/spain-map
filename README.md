# Spain Map component, v0.1

Tiny JS component which allows you to render an SVG Spain map, divided by provinces.
Using this component you can integrate interactive, colorful and localisable map of
Spain into your project. Component is strying to use user's navigator language for the
localization purposes, default language is Spanish.

See demo [here](https://psavinov.github.io/spain-map).

## Basic SpainMap usage example
```html
<html>
    <head>
        <meta charset="utf-8">
        <script src="spain-map-min.js"></script>
    </head>
    <body>
        <div id="mapTarget"></div>
    </body>
    <script>
        var map = new SpainMap(
            '#mapTarget',
            'es'
        );

        map.render();
    </script>
</html>
```

## SpainMap usage with plugins
Component support serial execution of as many plugins as developer wants.
Plugin allow to define custom behavior for *click*, *hover*, and *render* events.

### Custom plugin example:
```javascript
        var demoPlugin = new SpainMapPlugin(
            /* plugin ID, to make sure that same plugin applied only once */
            'demo', 
            {
                /* click event handler, per province  */
                click: function(event) {
                    this.setProvinceColor(
                        event.province.id, 
                        `rgb(${Math.random()*255},${Math.random()*255},${Math.random()*255})`
                    );
                },
                /* hover event handler, per province  */
                hover: function(event) {
                    this.highlightProvinceById(
                        event.province.id, 
                        `rgb(${Math.random()*255},${Math.random()*255},${Math.random()*255})`
                    );
                },
                /* render event handler, called once the map is rendered */
                render: function() {
                    console.log('Spain map rendered');
                }
            }
        );
```

### Custom event structure:
*click* and *hover* handlers accept *event* parameter with the following structure:
```javascript
{
    /* Selected autonomous community data */
    community,
    /* Selected provice data */
    provice,
    /* Original browser event */
    originalEvent
}
```

### Plugin usage example:
```html
<html>
    <head>
        <meta charset="utf-8">
        <script src="spain-map-min.js"></script>
    </head>
    <body>
        <div id="mapTarget"></div>
    </body>
    <script>
        var map = new SpainMap(
            '#mapTarget',
            'es'
        );

        var demoPlugin = new SpainMapPlugin(
            'demo',
            {
                click: function(event) {
                    this.setProvinceColor(
                        event.province.id, 
                        `rgb(${Math.random()*255},${Math.random()*255},${Math.random()*255})`
                    );

                    console.log(event.province.name[this.getLanguage()]);
                },
                hover: function(event) {
                    console.log(event.province.name[this.getLanguage()]);
                },
                render: function() {
                    console.log('Spain map rendered');
                }
            }
        );

        map.plugin(demoPlugin);

        map.render();
    </script>
</html>
```

## Available SpainMap and SpainMapPlugin instance members
#### Constructors:
```javascript
/**
 * Creates new SpainMap instance with xpecific node to render and language.
 * Target node can an HTML element, CSS selector or a function which returns an HTMLDivelement.
 * 
 * @param {HTMLElement|string|function} targetNode Node to render map in.
 * @param {string} language Language code.
 * @param {Array<SpainMapPlugin>} plugins Plugins to use with this instance.
 */
SpainMap = function (
    targetNode, 
    language = navigator.language || navigator.userLanguage || DEFAULT_LANGUAGE,
    plugins = []
){}

/**
 * Creates new SpainMapPlugin instance.
 * 
 * @param {string} id Plugin id.
 * @param {object<{
 *  click, hover, render
 * }>} events Events handlers (click, hover, render).
 */
SpainMapPlugin = function (
	id, 
	{
        click, hover, render
	}
){}
```

#### SpainMap Methods:
```javascript

interface Province {
    /* internal province id */
    id: string;
    /* localized name object, by language code */
    name: object;
}

/**
 * Returns the list of all the provinces.
 */
getAllProvinces(): Array<Province>;

/**
* Returns the list of provinces by autonomous community id.
* 
* @param {string} communityId Autonomous community id.
*/
getProvincesByCommunityId(communityId): Array<Province>;

/**
 * Returns the list of provinces by autonomous community name.
 * 
 * @param {string} communityName Autonomous community name.
 */
getProvincesByCommunityName(communityName): Array<Province>;

/**
 * Returns current language code.
 */
getLanguage(): string;

/**
 * Highlights selected autonomous community with specific color by id.
 * 
 * @param {string} communityId Autonomous community id.
 * @param {string} color Color name or HTML code.
 */
highlightCommunityById(communityId, color): void;

/**
 * Highlights selected autonomous community with specific color by name.
 * 
 * @param {string} communityName Autonomous community name.
 * @param {string} color Color name or HTML code.
 */
highlightCommunityByName(communityName, color): void;

/**
 * Highlights selected province with specific color by id.
 * 
 * @param {string} provinceId Province id.
 * @param {string} color Color name or HTML code.
 */
highlightProvinceById(provinceId, color): void;

/**
 * Highlights selected province with specific color by name.
 * 
 * @param {string} provinceName Province name.
 * @param {string} color Color name or HTML code.
 */
highlightProvinceByName(provinceName, color) : void;

/**
 * Plugs in new SpainMapPlugin instance
 * 
 * @param {SpainMapPlugin} plugin New plugin.
 */
plugin(plugin): void;

/**
 * Render a map in the target node.
 * 
 * @param {number} width Map width, default width 600px.
 * @param {number} height Map height, default height = 400px.
 * @param {number} scale Map scale, default scale = 1.
 */
render(width, height, scale = 1): void; 

/**
 * Sets selected autonomous community color by id.
 * 
 * @param {string} communityId Autonomous community id.
 * @param {string} color Color name or HTML code.
 */
setCommunityColor(communityId, color): void;

/**
 * Sets selected province color by id.
 * 
 * @param {string} provinceId Province id.
 * @param {string} color Color name or HTML code.
 */
setProvinceColor(provinceId, color): void;

/**
 * Sets Map scale.
 * 
 * @param {number} scale Map scale.
 */
setScale(scale): void
```

## Contribution
Feel free to contribute to this project, any help will be appreciated! ;)

Visit my [YouTube channel](http://youtube.com/PavelSavinovES) ;)
