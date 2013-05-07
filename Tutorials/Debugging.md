# Debugging

The Bifrost code is well tested, but from time there may be a need to step into the source from within your own solution. 

In most situations, you'd typically want to just grab the debug-symbols, step into the code and figure out whatever it is you're interested in. If you need to get more involved, then you should get the source directly.


## Using debug symbols
All official releases of Bifrost after v1.0.0.8 will have debug symbols available on [SymbolSource](http://www.symbolsource.org).

### Enable source server

To enable symbol server support within Visual Studio do the following  : 

 1. Tools > Options > Debugging > General
 	- Check "Enable source server support"
 
 2. Tools > Options > Debugging > Symbols
 	- Click the "new"-icon 
 	- Enter the following server : ``http://srv.symbolsource.org/pdb/Public``


 More resources : 
  - [SymbolSource.org](http://www.symbolsource.org)
  - [SymbolSource.org - Configuring Visual Studio](http://www.symbolsource.org/)


 ## Get the source code

 As mentioned earlier, you may want to get a bit more involved, and actually head over to the [Bifrost GitHub repository](http://github.com/dolittle/Bifrost) to get a given version of the code. The differnt versions are marked with tags, which should allow you to navigate between releases.