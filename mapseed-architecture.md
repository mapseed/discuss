# MapSeed Architecture
## Concept: What is MapSeed?
MapSeed is a tool to create an interactive website that allows users to give location-based feedback about their community on a map. Its target audience is companies, organizations, groups or individuals who want community input on something geographical in nature (parks in an area, or public utilities like street lights, for instance).

MapSeed should be:

- Easy to use: no technical or design skills necessary
- Easy to tweak: most look-and-feel and content changes are easy to make
- Deeply extensible: with a little tech savvy, it can be extended to new domains

Maps made with MapSeed should be:

- Nice-looking
- Informative: useful data can be visualized on map
- User friendly for community members: simple to report things so others can see them on the map
- User friendly for administrators: easy to manage and respond to reports

## How should MapSeed work for a map creator?
### Option 0
#### Setup
A map creator will download a Python/Django application. They will customize the look and feel of their website using config files they can edit in a text editor. If they haven't yet set up a Shareabouts backend, they need to go do that, then come back and enter its URL into their config. When they are done, they will deploy the Python/Django application to AWS/Azure/Google Compute.
#### Edit
The map creator will edit the config file further, copy/deploy that file somehow to the server, and restart the app.
#### Extend
##### Styles
The map creator (or a programmer they hire) will edit the CSS in the directory where it's stored.
##### Content
The map creator (or a programmer they hire) will edit the Django templates in the directory where they are stored.
##### Datastore
The map creator (or a programmer they hire) will edit the code (language?) of the Shareabouts backend.

### Option 1
#### Setup
A map creator will either download some software or navigate to a website. There they will set up their website in some kind of GUI, giving information about the look and feel and content of it. If they haven't yet set up a Shareabouts backend, they will set that up, and point their website to it. When they are done, they can download/output a Python/Django application for them to host on AWS/Azure/Google Compute.
#### Edit
The map creator will return to the software/website, make changes, and download/output a new version of the Django app.
#### Extend
##### Styles
The map creator will edit the CSS that they downloaded/output as part of the Django app.
##### Content
The map creator (or a programmer they hire) will edit the Django templates.
##### Datastore
The map creator (or a programmer they hire) will edit the code (language?) of the Shareabouts backend.

### Option 2
#### Setup
A map creator will either download some software or navigate to a website. There they will configure their Shareabouts backend with all of the aspects of their website: the title, pages/content, layers, etc. Then they will download a static site with only one configuration option, which is the backend that it points at. This static site will never change.
#### Edit
In order to update their site, the site creator will come back to MapSeed and reconfigure their backend.
#### Extend
##### Styles
The styles that are common to all MapSeed websites, and therefore in the static website, will be editable as CSS. Any way we have made the styles configurable from the backend (for example, the title color) will only be editable from there. If they wanted to make each word in their title a different color, to follow the example, they would have to do so in some very complicated, hacky way in the static site code.
##### Content
The elements of content that are common to all MapSeed websites (like the fact that they have a footer), and therefore in the static website, will be editable as HTML. Any way we have made the content configurable from the backend (for example, the pages on the website) will only be editable from there. If we only supported Markdown for those pages, to follow the example, they could not have video embeds without doing something very complicated and hacky in the static site code.
##### Datastore
If, as in the previous two examples, the map creator wanted to do things that don't fit within the MapSeed configuration, the best method would be to make code changes to the datastore. This would be accomplished the same way it was in the other options.

### Option 3
#### Setup
A map creator will either download some software or navigate to a website. They will be prompted by the software/website to input the title of their map, some layers of information to add to it, etc. If they have not already set up a Shareabouts backend, they will be prompted to create one as part of the setup process (either on their own servers or on the MapSeed servers, SaaS style). When they have finished, they can choose to download their static website to set up themselves on GitHub Pages, S3, or some other static hosting service (many of which are free or very cheap). They can also choose to host their site with MapSeed for some cost per month/year.
#### Edit
If the map creator want to update their site (add layers, change title, change pages or their contents, etc.) they will go back to MapSeed, make their changes, and either redownload or republish to our SaaS platform.
#### Extend
##### Styles
The map creator (or a programmer they hire) will edit the CSS they downloaded as part of their static website.
##### Content
The map creator (or a programmer they hire) will edit the HTML they downloaded as part of their static website.
##### Datastore
The map creator (or a programmer they hire) will edit the code (language?) of the Shareabouts backend.


## What do these options mean?
Option 0 is a description of how MapSeed/Smartercleanup currently works. That was given in order to compare with the other options.

Option 1 is a general picture of what MapSeed might look like if it stays in its current architecture, but becomes more user friendly. It is essentially a very polished version of how MapSeed/Smartercleanup currently works. Architecturally, MapSeed would be a webapp that would take configuration files.

Option 2 is a description of how MapSeed might work if we moved everything that is currently written in Python/Django into client-side Javascript. In this case, our Shareabouts backend becomes the main part the map creator configures. Architecturally, MapSeed would be a *single* static site that is hooked up to a very configurable backend.

Option 3 is a description of how MapSeed might work if we changed it from a webapp to a static site generator. This would still have a "server-side" component, but it would only be run once (as opposed to being constantly running like with Django). Architecturally, MapSeed would be a static site generator that would take configuration files.

## What are the concerns?
In my opinion, the high-level concerns (which of course can be subdivided further) are:
- Look and feel: the styles of the map, the icons used, etc.
- Static content/Background information: the global information about the subject matter (e.g. some content about the history of the Duwamish river or a page about what submissions the map is looking for)
- Dynamic outside content: layers/data coming from other sources
- User-generated content: what the users report

In any project, it is usually best to match the technical architecture to these high-level concerns.

### Option 0: separation of concerns
In the current state of MapSeed/Smartercleanup, here is how the concerns are divided up:
- Look and feel: Handled by the CSS of the Django app, CSS of the flavors (config).
- Static content/Background information: Stored in config files and parsed by the Django app, but Landmarks come from the Shareabouts backend.
- Dynamic outside content: Layers are stored in config files and parsed by a combination of the Django app and the client-side Javascript. They are then fetched via AJAX on the client.
- User-generated content: Handled by the Shareabouts backend.

### Option 1: separation of concerns
- Look and feel: Handled by the CSS of the Django app, CSS of the flavors (config).
- Static content/Background information: Stored in config files and parsed by the Django app. Let's say by then we will be storing Landmarks in the config as well.
- Dynamic outside content: Let's say the layers are parsed from config entirely by the Django app. They would then be fetched via AJAX on the client.
- User-generated content: Handled by the Shareabouts backend.

### Option 2: separation of concerns
- Look and feel: CSS of the static site in combination with (probably) certain stylings that would come from the Shareabouts backend.
- Static content/Background information: Stored in the Shareabouts backend.
- Dynamic outside content: Layers stored in the Shareabouts backend, outside data fetched via AJAX.
- User-generated content: Handled by the Shareabouts backend.

### Option 3: separation of concerns
- Look and feel: CSS of the static site.
- Static content/Background information: HTML of the static site.
- Dynamic outside content: Layers stored in Javascript, outside data fetched via AJAX.
- User-generated content: Handled by the Shareabouts backend.

## What are the performance implications?

Option 0 and Option 1 both require Python to be constantly running on the server side. This will cause a slight performance overhead by the creation of each page, but also means that map creators cannot take advantage of a CDN, which will have large performance impacts.

Option 2 requires Javascript to run and AJAX requests to be made before even the most basic functionality exists (for example, the title of the page). While this will probably not be the bottleneck, it also requires a database hit on the Shareabouts server before the most basic functionality exists.

Option 3, in my opinion, is the best of both worlds: like Option 2, it generates a static site that can be deployed very efficiently using a CDN, but that site will "come with" most of the content on the first roundtrip. The only pieces requiring a second roundtrip will be the map tiles, the external data layers, and the Shareabouts places.
