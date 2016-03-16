
#Browser-solidity

Browser solidity is a browser based solidity compiler. To use either visit [https://chriseth.github.io/browser-solidity](https://chriseth.github.io/browser-solidity) or clone/download this repo and open `index.html` in your browser.

#Nodejs usage

To use the solidity compiler via nodejs you can install it via npm

	npm install solc

And then use it like so:

	var solc = require('solc');
	var input = "contract x { function g() {} }";
	var output = solc.compile(input, 1); // 1 activates the optimiser
	for (var contractName in output.contracts) {
		// code and ABI that are needed by web3
		console.log(contractName + ': ' + output.contracts[contractName].bytecode);
		console.log(contractName + '; ' + JSON.parse( output.contracts[contractName].interface));
	}

Starting from version 0.1.6, multiple files are supported with automatic import resolution by the compiler as follows:

	var solc = require('solc');
	var input = {
		'lib.sol': 'library L { function f() returns (uint) { return 7; } }',
		'cont.sol': 'import "lib.sol"; contract x { function g() { L.f(); } }'
	};
	var output = solc.compile({sources: input}, 1);
	for (var contractName in output.contracts)
		console.log(contractName + ': ' + output.contracts[contractName].bytecode);

Note that all input files that are imported have to be supplied, the compiler will not load any additional files on its own.

###Using a legacy version

In order to allow compiling contracts using a specific version of solidity, the `solc.useVersion` method is available. This returns a new solc object using the version provided. **Note**: version strings must match the version substring of the files availble in `/bin/soljson-*.js`. See below for an example.

	var solc = require('solc');
	// by default the latest version is used
	// ie: solc.useVersion('latest')

	// getting a legacy version
	var solcV011 = solc.useVersion( 'v0.1.1-2015-08-04-6ff4cd6' );
	var output = solcV011.compile( "contract t { function g() {} }", 1 );


#Using as a chrome extension

Extensions that you download from the Chrome Web Store are packaged up as .crx files, which is great for distribution, but not so great for development. Recognizing this, Chrome gives you a quick way of loading up your working directory for testing. Let's do that now.

Visit chrome://extensions in your browser (or open up the Chrome menu by clicking the icon to the far right of the Omnibox:  The menu's icon is three horizontal bars. and select Extensions under the Tools menu to get to the same place).

Ensure that the Developer mode checkbox in the top right-hand corner is checked.

Click Load unpacked extension… to pop up a file-selection dialog.

Navigate to the directory in which your extension files live, and select it.

Alternatively, you can drag and drop the directory where your extension files live onto chrome://extensions in your browser to load it.
