<script>
  let count = 0
  let elementCounter = 0;

  let words = ["Operational parameters updated",
  "System integrity remains stable",
  "Critical failure in sector 4",
  "Data breach detected",
  "Emergency shutdown initiated",
  "Security protocol activated",
  "Maintenance required in subsystem 3",
  "Network connection unstable",
  "Hardware malfunction reported",
  "Software update available",
  "Backup power supply online",
  "Firewall breach attempt detected",
  "System diagnostic completed",
  "Database synchronization failed",
  "Encryption key updated",
  "User access level changed",
  "Server overload warning",
  "Data recovery in progress",
  "System reboot required",
  "Virus scan initiated",
  "Intrusion detection system activated",
  "Power surge detected",
  "Hardware upgrade required",
  "Software patch applied successfully",
  "System resources at maximum capacity",
  "Data transfer rate decreased",
  "Battery level low",
  "Temperature warning in server room",
  "Unexpected error occurred",
  "System performance optimized",
  "Login attempt failed",
  "Password reset required",
  "Email server offline",
  "Data encryption in progress",
  "System shutdown in 3... 2... 1...",
  "Running system health check",
  "Restoring factory settings",
  "Updating system firmware",
  "Loading system preferences",
  "Establishing secure connection",
  "Scanning for network devices",
  "Saving changes to configuration",
  "System update postponed",
  "Restarting network adapter",
  "Running disk cleanup",
  "Checking for software updates",
  "Scanning for malware",
  "Optimizing system performance",
  "Checking disk for errors",
  "Restoring previous version",
  "Installing device driver software",
  "Preparing to configure system",
  "Don't turn off your computer","apple", "banana", "cherry", "date", "elderberry", "fig", "grape", "honeydew", "iceberg", "jackfruit", "kiwi", "lemon", "mango", "nectarine", "orange", "pineapple", "quince", "raspberry", "strawberry", "tangerine"];

  let elements = ["p", "div", "input"];

  let attributesMap = {
    "p": [
      {
        attributeName: "textContent",
        attributeValues: words
      },
    ],
    "div": [
      {
        attributeName: "textContent",
        attributeValues: words
      }
    ],
    "input": [
      {
        attributeName: "placeholder",
        attributeValues: words
      },
      {
        attributeName: "type",
        attributeValues: ["password", "checkbox", "radio", "submit", "reset", "file", "hidden", "image", "button", "color", "date", "datetime-local", "email", "month", "number", "range", "search", "tel", "time", "url", "week"]
      },
      {
        attributeName: "value",
        attributeValues: words
      },
      {
        attributeName: "name",
        attributeValues: words
      },
      {
        attributeName: "disabled",
        attributeValues: ["disabled"]
      },
      {
        attributeName: "readonly",
        attributeValues: ["readonly"]
      },
      {
        attributeName: "required",
        attributeValues: ["required"]
      },
      {
        attributeName: "maxlength",
        attributeValues: ["10", "20", "30"]
      },
      {
        attributeName: "min",
        attributeValues: ["1", "5", "10"]
      },
      {
        attributeName: "max",
        attributeValues: ["100", "200", "300"]
      },
      {
        attributeName: "pattern",
        attributeValues: ["[A-Za-z]", "\\d", "[A-Za-z0-9]"]
      },
      {
        attributeName: "autocomplete",
        attributeValues: ["on", "off"]
      },
      {
        attributeName: "autofocus",
        attributeValues: ["autofocus"]
      },
      {
        attributeName: "size",
        attributeValues: ["10", "20", "30"]
      },
      {
        attributeName: "step",
        attributeValues: ["1", "2", "3"]
      },
      {
        attributeName: "list",
        attributeValues: ["datalist1", "datalist2", "datalist3"]
      },
      {
        attributeName: "multiple",
        attributeValues: ["multiple"]
      },
      {
        attributeName: "form",
        attributeValues: ["form1", "form2", "form3"]
      },
      {
        attributeName: "src",
        attributeValues: ["image1.jpg", "image2.jpg", "image3.jpg"]
      },
      {
        attributeName: "alt",
        attributeValues: ["Image 1", "Image 2", "Image 3"]
      },
      {
        attributeName: "accept",
        attributeValues: [".jpg", ".png", ".gif"]
      },
      {
        attributeName: "checked",
        attributeValues: ["checked"]
      },
      {
        attributeName: "autocapitalize",
        attributeValues: ["none", "sentences", "words", "characters"]
      },
      {
        attributeName: "inputmode",
        attributeValues: ["verbatim", "latin", "latin-name", "latin-prose", "full-width-latin", "kana", "katakana", "numeric", "tel", "email", "url"]
      }
    ]
  }

  let elementIds = [];

  let getScale = (element) => {
    const style = window.getComputedStyle(element);
    const transform = style.transform;

    // If the transform property is 'none', the element has not been scaled
    if (transform === 'none') {
      return 1;
    }

    // The transform property is a matrix transformation in the form
    // matrix(scaleX(), skewY(), skewX(), scaleY(), translateX(), translateY())
    // We can split this string to get the scale values
    const values = transform.split('(')[1].split(')')[0].split(',');
    const scaleX = parseFloat(values[0]);

    // If the element is uniformly scaled, scaleX and scaleY will be the same
    // If not, you can return an object with both values
    return scaleX;
  }

  let removeElementsByClass = (className) => {
    let elements = document.getElementsByClassName(className);
    while(elements.length > 0){
      elementIds = elementIds.filter((id) => id !== elements[0].id);
      elements[0].parentNode.removeChild(elements[0]);
    }
  }

  let collide = (rect1, rect2) => {
    return !(
      rect1.top > rect2.bottom ||
      rect1.right < rect2.left ||
      rect1.bottom < rect2.top ||
      rect1.left > rect2.right
    );
  }

  let inside = (rect1, rect2) => {
    return (
      ((rect2.top <= rect1.top) && (rect1.top <= rect2.bottom)) &&
      ((rect2.top <= rect1.bottom) && (rect1.bottom <= rect2.bottom)) &&
      ((rect2.left <= rect1.left) && (rect1.left <= rect2.right)) &&
      ((rect2.left <= rect1.right) && (rect1.right <= rect2.right))
    );
  }

  let containsClickCenter = (rect, clickCenter) => {
    return (
      (rect.top <= clickCenter.y) && (clickCenter.y <= rect.bottom) &&
      (rect.left <= clickCenter.x) && (clickCenter.x <= rect.right)
    );
  }

  let normalizeVector = (vector) => {
    let magnitude = Math.sqrt(vector.x**2 + vector.y**2);
    return {
      x: vector.x / magnitude,
      y: vector.y / magnitude
    };
  }

  let explode = (event, element) => {
    const mainRect = element.getBoundingClientRect();

    let explosionVector;
    let normalizedExplosionVector;
    let explosionDistance = 2 * Math.max(window.innerWidth, window.innerHeight);
    
    let windowCenter = {
      x: window.innerWidth/2,
      y: window.innerHeight / 2
    };
    let mainCenter = {
      x: mainRect.x + mainRect.width / 2,
      y: mainRect.y + mainRect.height / 2
    };
    let clickCenter = {
      x: event.clientX,
      y: event.clientY
    };

    elementIds.forEach((elementId) => {
      let secondaryElement = document.getElementById(elementId);
      let secondaryRect = secondaryElement.getBoundingClientRect();

      if (elementId !== element.id && containsClickCenter(secondaryRect, clickCenter)) {
        // Get the center of the secondaryRect
        let secondaryCenter = {
          x: secondaryRect.x + secondaryRect.width / 2,
          y: secondaryRect.y + secondaryRect.height / 2
        };
        // Determine the vector between the two centers
        explosionVector = {
          x: secondaryCenter.x - clickCenter.x,
          y: secondaryCenter.y - clickCenter.y
        };
        normalizedExplosionVector = normalizeVector(explosionVector);
        // Move away the second element from the first element outside of the viewport in the direction of the explosion vector
        
        secondaryElement.style.transition = "all 0.5s ease";
        secondaryElement.style.transform = `translate(${normalizedExplosionVector.x*explosionDistance}px, ${normalizedExplosionVector.y*explosionDistance}px)`; 
        secondaryElement.classList.add("remove");

      }
    });

    explosionVector = {
      x: mainCenter.x - clickCenter.x,
      y: mainCenter.y - clickCenter.y
    };
    normalizedExplosionVector = normalizeVector(explosionVector);

    element.style.transition = "all 0.5s ease";
    element.style.transform = `translate(${normalizedExplosionVector.x*explosionDistance}px, ${normalizedExplosionVector.y*explosionDistance}px)`; 
    element.classList.add("remove");
  }


  function generateStyle(element) {

    if (Math.random() < 0.9) element.style['background-color'] = `rgb(${Math.random()*255},${Math.random()*255},${Math.random()*255})`;
    if (Math.random() < 0.6) element.style['color'] = `rgb(${Math.random()*255},${Math.random()*255},${Math.random()*255})`;
    if (Math.random() < 0.2) element.style['size'] = Math.random() * 100;
    if (Math.random() < 0.05) element.style['margin'] = Math.random() * 100;element.style['border-radius'] = `${Math.random() * 100}%`;
    if (Math.random() < 0.6) element.style['border-color'] = `rgb(${Math.random()*255},${Math.random()*255},${Math.random()*255})`;
    if (Math.random() < 0.6) element.style['border-width'] = `${Math.random() * 10}px`;
    if (Math.random() < 0.2) element.style['padding'] = `${Math.random() * 50}px`;
    if (Math.random() < 0.2) element.style['font-size'] = `${Math.random() * 30}px`;
    if (Math.random() < 0.2) element.style['z-index'] = `${Math.floor(Math.random() * 1000)}`;
    if (Math.random() < 0.2) element.style['box-shadow'] = `${Math.random() * 10}px ${Math.random() * 10}px ${Math.random() * 20}px rgba(0, 0, 0, ${Math.random()})`;
    if (Math.random() < 0.2) element.style['opacity'] = `${Math.random()}`;
    if (Math.random() < 0.2) element.style['filter'] = `blur(${Math.random() * 10}px)`;
    if (Math.random() < 0.8) element.style['clip-path'] = `circle(${Math.random() * 100}% at ${Math.random() * 100}% ${Math.random() * 100}%)`;
    if (Math.random() < 0.2) element.style['transform'] = `scale(${Math.random() * 10})`;
  }

  let positionElement = (element) => {
    let x = Math.floor(Math.random() * window.innerWidth);
    let y = Math.floor(Math.random() * window.innerHeight);

    element.style.display = "block";
    element.style.position = "absolute";
    element.style.left = x + "px";
    element.style.top = y + "px";
  }

  const addElements = () => {
    const elementType = elements[Math.floor(Math.random() * elements.length)];
    const parentElementId = "other-content-container";
    elementCounter += 1;
    const elementId = 'element-' + (elementCounter + 1);
    elementIds = [...elementIds, elementId];

    // Create and add the element
    let newElement = document.createElement(elementType); // create new element
    document.getElementById(parentElementId).appendChild(newElement); // append new element

    // Set the attributes
    newElement.id = elementId;
    // if (elementType === "p") debugger;
    attributesMap[elementType].forEach(({attributeName, attributeValues}) => {
      if (newElement[attributeName] || newElement[attributeName] === "") {
        newElement[attributeName] = attributeValues[Math.floor(Math.random() * attributeValues.length)];
      }
    });

    newElement.addEventListener('pointerup', (event) => {
      removeElementsByClass("remove");
      explode(event, newElement);
    });
    
    newElement.addEventListener('pointermove', (event) => {
      newElement.style['transform'] = `scale(${(getScale(newElement) + 20) / 2})`;
    });

    // Position the element
    positionElement(newElement);
    generateStyle(newElement);
    newElement.style.transition = "all 0.3s ease";
  }

  const updateElements = () => {
    elementIds.forEach((elementId) => {
      if (Math.random() < 0.2) {
        let element = document.getElementById(elementId);
        generateStyle(element);
        positionElement(element);
      }
    });
  }

  const removeElements = () => {
    elementIds.forEach((elementId) => {
      if (Math.random() < 0.1) {
        let element = document.getElementById(elementId);
        element.remove();
        elementIds = elementIds.filter((id) => id !== elementId);
      }
    });
  }
  
  
  const increment = () => {
    removeElementsByClass("remove");

    count += 1
    elementCounter += count;

    for(let i = 0; i <= count; i++) {
      addElements();
    }
    updateElements();
    removeElements();
    
    // positionElement(document.getElementById('counter-container'));
  }
</script>

<button on:click={increment}>
  Interactive
</button>
