# A simple SBGN-ML renderering API

This application is a simple REST API to render SBGN-ML files.

It is based a stripped down version of [Newt](https://github.com/iVis-at-Bilkent/newt), which is executed from Selenium using chromedriver. It comes with a simple python library which implements this wrapper and can be used independently of the API.

## Run with docker and docker-compose

```
git clone https://github.com/vincent-noel/newt.git
cd newt
sudo docker-compose up -d
```

## Using the API

Endpoint : http://localhost:8082/render

Method : POST

Files: 
  - file : the sbgn-ml file to render
  
Parameters: 
  
  - format : image format (svg|png|jpg) (optional, default=png)
  - scale : scaling value (optional, default 3 for jpg/png, 1 for svg)
  - max_width : maximum width (optional, default depends on scale)
  - max_height : maximum height (optional, default depends on scale)
  - quality : JPG quality (optional, default 1, only for jpg)
  
Returns: 
  - rendered image

Example using python
```
import requests

files = {'file': open('sbgnml.xml','rb')}
values = {
    'format': 'png',
    'bg': '#fff',
    'scale': 1,
    'max_width': 5000,
    'max_height': 5000,
}
api_url = "http://localhost:8082/render"

r = requests.post(api_url, files=files, data=values)
with open('network.png', 'wb') as f:
    f.write(r.content)

```

A public rendering API is available at 
```
http://sbgnrender.vincent.science/render
```

## Installation
In order to deploy and run a local instance of the tool, please follow the steps below (we recommend the use of LTS version 12.16.1 of node.js):

- Installation
```
git clone https://github.com/vincent-noel/newt.git
cd newt/sbgnrender
npm install  
```

- Running the server
```
cd ..
python3 api/RendererAPI.py

```

Then, open a web browser and navigate to 
```
http://localhost?url=\<the url of the sbgn-ml file\>
```
After rendering, the file will start downloading.
Please note that the default port is 80 but you might have to run this application in another port such as 8080 in some platforms by setting 'port' environment variable.

# Python library
This API is build with Flask and uses a python library. 
To install it
```
git clone https://github.com/vincent-noel/newt.git
pip3 install newt/
``` 

This library depends on ChromeDriver, which can be installed on ubuntu/debian systems with 
```
sudo apt install chromium-chromedriver
```

On Ubuntu 19.x and later, chromium installation is using snap package manager, which causes a problem while writing and accessing temporary files. The current workaround is to change chromium temporary directory permissions with : 
```
sudo chmod 711 /tmp/snap.chromium
```

After installing the library, usage is the following : 
```
from sbgnrender import renderSBGN

renderSBGN(
    input_file,  // Path as a string
    output_file, // Path as a string
    format,      // Format as a string : svg, png, jpg
    scale,       // Scale of the network (default 1 for svg, 3 for jpg/png)
    bg,          // Background color as HTML String (ex #fff for white), None for transparent (available for png/svg),
    max_width,   // Maximum width in pixels
    min_width,   // Minimum width in pixels
    quality,     // Quality (available for jpg)
    verbose      // True | False
)
```

## Software

This application is distributed under [GNU Lesser General Public License](http://www.gnu.org/licenses/lgpl.html).

## Copyright

A Simple SBGN Rendering API, Copyright (C) 2020 Institut Curie, 26 rue d'Ulm, Paris, France

A Simple SBGN Rendering API is free software; you can redistribute it and/or modify it under the terms of the GNU Lesser General Public License as published by the Free Software Foundation; either version 3 of the License, or (at your option) any later version.

A Simple SBGN Rendering API is distributed in the hope that it will be useful, but WITHOUT ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the GNU Lesser General Public License for more details.

You should have received a copy of the GNU Lesser General Public License along with this library; if not, write to the Free Software Foundation, Inc., 51 Franklin Street, Fifth Floor, Boston, MA 02110-1301, USA