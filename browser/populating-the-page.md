# Populating the page: how browsers work

Two major issues in web performance are issues having to do with __latency__ and issues having to do with the fact that for the most part, browsers are __single-threaded__.

1. DNS lookup

    1. Client queries local DNS cache

    2. If the IP adress in not in the cache, client queries the Recursive DNS Servers (DNS resolvers), otherwise it uses the cached value

    3. If the Recursive DNS Server doesn't have the answer, it queries the Authoritative DNS Servers, otherwise it send back the answer

    4. Recursive DNS Server gets the A record and stores with as long as it's defined with TTL

    5. Client gets the answer

2. TCP handshake (SYN, SYN-ACK, ACK)

    This is required to establish a TCP connection.
    This requires __3__ messages between the client and the server.

3. TLS negotiation

    This is required to establish a secure connection.
    This requires __5__ more messages.

4. First response - __TTFB__

    Client gets the first HTTP response from the server which includes usually a HTML file and response headers.

5. Parsing

    Parsing is the step the browser takes to turn the data it receives over the network into the __DOM__ and __CSSOM__, which is used by the renderer to paint a page to the screen.

    1. Building the DOM tree

        1. Tokenization

            Tokens include start and end tags as well as attribute names and values

        2. Building the document tree
            The parser parses tokenized input into the document, building up the document tree.

            The DOM tree describes the content of the document. The `<html>` element is the first element and root node of the document tree.

            When the parser finds non-blocking resources, such as an image, the browser will request those resources and continue parsing. Parsing can continue when a CSS file is encountered, __but `<script>` elements — particularly those without an `async` or `defer` attribute — block rendering, and pause the parsing of HTML.__

    2. Building the CSSOM tree

        The CSS object model is similar to the DOM. The DOM and CSSOM are both trees. __They are independent data structures.__ The browser converts the CSS rules into a map of styles it can understand and work with. The browser goes through each rule set in the CSS, creating a tree of nodes with parent, child, and sibling relationships based on the CSS selectors.

        Building the CSSOM is very, very fast and is not displayed in a unique color in current developer tools. In terms of web performance optimization, there are lower hanging fruit, as the __total time to create the CSSOM is generally less than the time it takes for one DNS lookup__.

    3. Preload scanner

        While the browser builds the DOM and CSSOM trees, this process occupies the main thread. As this happens, the preload scanner will parse through the content available and request high-priority resources like CSS, JavaScript, and web fonts.

    4. JavaScript compilation

        JavaScript is parsed, compiled, and interpreted. The scripts are parsed into __abstract syntax trees__. Some browser engines take the abstract syntax trees and pass them into a compiler, outputting bytecode. This is known as JavaScript compilation. Most of the code is interpreted on the main thread, but there are exceptions such as code run in web workers.

    5. Building the accessibility tree (AOM)

6. Rendering

    Rendering steps include style, layout, paint, and in some cases compositing. The CSSOM and DOM trees created in the parsing step are combined into a render tree which is then used to compute the layout of every visible element, which is then painted to the screen. In some cases, content can be promoted to its own layer and composited, improving performance by painting portions of the screen on the GPU instead of the CPU, freeing up the main thread.

    1. Style

        The third step in the critical rendering path is combining the DOM and CSSOM into a render tree. The computed style tree, or render tree, construction starts with the root of the DOM tree, traversing each visible node.

        Elements that aren't going to be displayed, like the `<head>` element and its children and any nodes with `display: none`, such as the `script { display: none; }` you will find in user agent stylesheets, are not included in the render tree as they will not appear in the rendered output. Nodes with `visibility: hidden` applied are included in the render tree, as they do take up space.

        Each visible node has its CSSOM rules applied to it. The render tree holds all the visible nodes with content and computed styles.

    2. Layout

        The fourth step in the critical rendering path is running layout on the render tree to compute the geometry of each node. Layout is the process by which the dimensions and location of all the nodes in the render tree are determined, plus the determination of the size and position of each object on the page. __Reflow__ is any subsequent size and position determination of any part of the page or the entire document.

        To determine the exact size and position of each object, the browser starts at the root of the render tree and traverses it.

        On the web page, almost everything is a box. Different devices and different desktop preferences mean an unlimited number of differing viewport sizes. Taking the viewport size into consideration, the browser determines what the sizes of all the different boxes are going to be. Taking the size of the viewport as its base, layout generally starts with the body, laying out the sizes of all the body's descendants.

        The first time the size and position of each node is determined is called layout. Subsequent recalculations of are called reflows. In our example, suppose the initial layout occurs before the image is returned. Since we didn't declare the dimensions of our image, there will be a reflow once the image dimensions are known.

    3. Paint

        The last step in the critical rendering path is painting the individual nodes to the screen, the first occurrence of which is called the __first meaningful paint__. The browser converts each box calculated in the layout phase to actual pixels on the screen.

        To ensure smooth scrolling and animation, everything occupying the main thread, including calculating styles, along with reflow and paint, must take the browser less than __16.67ms__ to accomplish.

        Painting can break the elements in the layout tree into layers. __Promoting content into layers on the GPU (instead of the main thread on the CPU) improves paint and repaint performance.__ There are specific properties and elements that instantiate a layer, including `<video>` and `<canvas>`, and any element which has the CSS properties of `opacity`, a 3D `transform`, `will-change`, and a few others. 

    4. Compositing

        When sections of the document are drawn in different layers, overlapping each other, compositing is necessary to ensure they are drawn to the screen in the right order and the content is rendered correctly.

7. Interactivity

    If the load includes JavaScript, that was correctly deferred, and only executed after the `onload` event fires, the main thread might be busy, and not available for scrolling, touch, and other interactions.

    __Time to Interactive (TTI)__ tells us how long it took from the first request to when the page is interactive — interactive being the point in time after the __First Contentful Paint__ when the page responds to user interactions within 50ms. __If the main thread is occupied parsing, compiling, and executing JavaScript, it is not available and therefore not able to respond to user interactions in a timely (less than 50ms) fashion.__