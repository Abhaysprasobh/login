"use strict";

var styles = `
  .flutter-loader {
    width: 100%;
    height: 8px;
    background-color: #13B9FD;
    position: absolute;
    top: 0px;
    left: 0px;
    overflow: hidden;
  }

  .indeterminate {
      position: relative;
      width: 100%;
      height: 100%;
  }

  .indeterminate:before {
      content: '';
      position: absolute;
      height: 100%;
      background-color: #0175C2;
      animation: indeterminate_first 2.0s infinite ease-out;
  }

  .indeterminate:after {
      content: '';
      position: absolute;
      height: 100%;
      background-color: #02569B;
      animation: indeterminate_second 2.0s infinite ease-in;
  }

  @keyframes indeterminate_first {
      0% {
          left: -100%;
          width: 100%;
      }
      100% {
          left: 100%;
          width: 10%;
      }
  }

  @keyframes indeterminate_second {
      0% {
          left: -150%;
          width: 100%;
      }
      100% {
          left: 100%;
          width: 10%;
      }
  }
`;

var styleSheet = document.createElement("style")
styleSheet.type = "text/css";
styleSheet.innerText = styles;
document.head.appendChild(styleSheet);

var loader = document.createElement('div');
loader.className = "flutter-loader";
document.body.append(loader);

var indeterminate = document.createElement('div');
indeterminate.className = "indeterminate";
loader.appendChild(indeterminate);

document.addEventListener('dart-app-ready', function (e) {
   loader.parentNode.removeChild(loader);
   styleSheet.parentNode.removeChild(styleSheet);
});


// A map containing the URLs for the bootstrap scripts in debug.
let _scriptUrls = {
  "mapper": "stack_trace_mapper.js",
  "requireJs": "require.js"
};

// Create a TrustedTypes policy so we can attach Scripts...
let _ttPolicy;
if (window.trustedTypes) {
  _ttPolicy = trustedTypes.createPolicy("flutter-tools-bootstrap", {
    createScriptURL: (url) => {
      let scriptUrl = _scriptUrls[url];
      if (!scriptUrl) {
        console.error("Unknown Flutter Web bootstrap resource!", url);
      }
      return scriptUrl;
    }
  });
}

// Creates a TrustedScriptURL for a given `scriptName`.
// See `_scriptUrls` and `_ttPolicy` above.
function getTTScriptUrl(scriptName) {
  let defaultUrl = _scriptUrls[scriptName];
  return _ttPolicy ? _ttPolicy.createScriptURL(scriptName) : defaultUrl;
}

// Attach source mapping.
var mapperEl = document.createElement("script");
mapperEl.defer = true;
mapperEl.async = false;
mapperEl.src = getTTScriptUrl("mapper");
document.head.appendChild(mapperEl);

// Attach require JS.
var requireEl = document.createElement("script");
requireEl.defer = true;
requireEl.async = false;
requireEl.src = getTTScriptUrl("requireJs");
// This attribute tells require JS what to load as main (defined below).
requireEl.setAttribute("data-main", "main_module.bootstrap");
document.head.appendChild(requireEl);
