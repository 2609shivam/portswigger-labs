# Client-side Prototyep Pollution via Browser APIs

## 📌 Lab Details
- **Title**: Client-side Prototype Pollution via Browser APIs
- **Difficulty**: Practitioner
- **Category**: Prototype Pollution
- **Lab URL**: https://portswigger.net/academy/labs/launch/9f29effeafaa743b0f87d5682f68184437df0b23db7ce32b2377869a95045976?referrer=%2fweb-security%2fprototype-pollution%2fclient-side%2fbrowser-apis%2flab-prototype-pollution-client-side-prototype-pollution-via-browser-apis

## 🔍 Summary
This lab demonstrates how client-side prototype pollution can be used to achieve DOM-based XSS through browser APIs. <br>
The application parses URL parameters into JavaScript objects without properly filtering the `__proto__` property, allowing an attacker to modify `Object.prototype`. <br>
Although the developers attempted to secure the vulnerable gadget by making the `transport_url` property non-writable and non-configurable using `Object.defineProperty()`, they forgot to explicitly define its `value`. Because JavaScript property lookups fall back to the prototype chain, polluting `Object.prototype.value`allows complete control over the script source, ultimately resulting in arbitrary JavaScript execution.

## 🛠 Steps to Solve
### Find a prototype pollution source
1. In your browser, try polluting `Object.prototype` by injecting an arbitrary property via the query string:
   ```sh
   /?__proto__[foo]=bar
   ```
2. Open the browser DevTools panel and go to the **Console** tab.
3. Enter `Object.prototype`.
4. Study the properties of the returned object and observe that your injected `foo` property has been added. You've successfully found a prototype pollution source.

### Identify a gadget
1. In the browser DevTools panel, go to the **Sources** tab.
2. Study the JavaScript files that are loaded by the target site and look for any DOM XSS sinks.
3. In `searchLoggerConfigurable.js`, notice that if the `config` object has a `transport_url` property, this is used to dynamically append a script to the DOM.
4. Observe that a `transport_url` property is defined for the `config` object, so this doesn't appear to be vulnerable.
5. Observe that the next line uses the `Object.defineProperty()` method to make the `transport_url` unwritable and unconfigurable. However, notice that it doesn't define a `value` property.

### Craft an exploit
1. Using the prototype pollution source you identified earlier, try injecting an arbitrary value property:
   ```sh
   /?__proto__[value]=foo
   ```
2. In the browser DevTools panel, go to the **Elements** tab and study the HTML content of the page. Observe that a `<script>` element has been rendered on the page, with the `src` attribute `foo`.
3. Modify the payload in the URL to inject an XSS proof-of-concept. For example, you can use a `data:` URL as follows:
   ```sh
   /?__proto__[value]=data:,alert(1);
   ```
4. Observe that the `alert(1)` is called and the lab is solved.

## 📖 Key Takeaways
- Client-side prototype pollution can modify `Object.prototype` through unsafe URL parsing.
- JavaScript property lookups traverse the prototype chain when an own property has no value.
- Browser APIs that dynamically create `<script>` elements are powerful XSS gadgets.
- `data:` URLs can be abused to execute arbitrary JavaScript when used as a script source.

## 🖼️ Screenshot 
<img width="960" height="540" alt="image" src="https://github.com/user-attachments/assets/b56f4eb1-39fb-43c4-b8f3-673562e5026e" />
<img width="960" height="540" alt="image" src="https://github.com/user-attachments/assets/5a3858f9-be72-4e0b-bfd4-8dde0d5aa5e6" />
<img width="960" height="540" alt="image" src="https://github.com/user-attachments/assets/eac443aa-6db9-445f-89ff-6865e5573678" />
<img width="960" height="540" alt="image" src="https://github.com/user-attachments/assets/026fa1f4-d879-42bd-b6c7-5a377f2806ef" />
<img width="960" height="540" alt="image" src="https://github.com/user-attachments/assets/97f153bd-d630-40f4-bec3-76177d124526" />
