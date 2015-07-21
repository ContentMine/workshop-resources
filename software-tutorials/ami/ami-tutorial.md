![ContentMine logo](https://github.com/ContentMine/assets/blob/master/png/Content_mine(small).png)

## What is ami?

ami is a collection of plugins that .... The better question would be: What do the different plugins do?


[1. Installation](#installation)



### Installation

norma can be installed from the latest `.deb`-file, [Download here](https://jenkins.ch.cam.ac.uk/view/AMI2/job/norma/org.xml-cml$norma/lastSuccessfulBuild/artifact/org.xml-cml/norma/0.1-SNAPSHOT/norma-0.1-SNAPSHOT.deb)

```
wget https://jenkins.ch.cam.ac.uk/view/AMI2/job/norma/org.xml-cml$norma/lastSuccessfulBuild/artifact/org.xml-cml/norma/0.1-SNAPSHOT/norma-0.1-SNAPSHOT.deb
sudo dpkg -i norma-0.1-SNAPSHOT.deb
# the password is password
```

The tutorial has been developed on 
```
$ norma --version
norma(0.1.8)
```

### How to use ami-plugins

### How to interpret ami-results

#### ami2-bagOfWords


#### ami2-species


minimum
```bash
$ ami2-species -q dinosaurs-xmls/ -i scholarly.html --sp.species
```

options
```
--sp.type genus binomial genussp
```

#### ami2-regex


How to construct a regex query

https://github.com/ContentMine/ami/blob/master/regex/agriculture.xml


#### ami2-gene


only type human available at the moment
```bash
ami2-gene -q dinosaurs-xmls/ -i scholarly.html --g.gene --g.type human
```

#### ami2-sequence



### Next steps


**Next steps**
* Continue to [sHTML](../sHTML/sHTML-overview.md) if you want to learn more about scholarly HTML.
* Continue to [ami](../norma/ami-tutorial.md) for the next step of the ContentMine pipeline.


<html>
   <head>
      <meta http-equiv="Content-Type" content="text/html; charset=UTF-8"><style>
					* {
					bold : bold;
					}
					code {
					font-family : courier;
					font-weight :
					bold;
					font-size : 14pt;
					}
				</style></head>
   <body>
      <div>
         <h1><tt>norma
               					(
               					0.1.7
               					)
               				</tt></h1>
         <h2>Arguments</h2>
         <ul>
            <li>
               <h2><tt>char</tt></h2><code>--chars [pairs of characters, as unicode points; i.e. char00, char01, char10, char11, char20,
                  char21{1,*}</code><br><em>Methods: </em> parse: parseChars<br><br><em>  b: </em><code>-c</code><em>  c: </em><code>org.xmlcml.cmine.args.StringPair</code><p>
                  <h3>Description</h3>
                  <help>
                     			CHARS:
                     			NOT YET IMPLEMENTED
                     			Replaces one character by another. There are many cases where original
                     			characters are unsuitable
                     			for science and can be replaced (often by low codepoints).
                     			Smart (balanced) quotes can usually be replaced by \" or '
                     			mdash is often used where minus is better
                     			Format, strings of form u1234
                     		
                  </help>
               </p>
            </li>
            <li>
               <h2><tt>div</tt></h2><code>--divs expression [expression] ...{1,*}</code><br><em>Methods: </em> parse: parseDivs<br><br><em>  b: </em><code>-d</code><em>  c: </em><code></code><p>
                  <h3>Description</h3>
                  <help>
                     			DIVS:
                     			NOT YET IMPLEMENTED
                     			List of expressions identifying XML element to wrap as divs/sections
                     			Examples might be h1, h2, h3, or numbers sections
                     			Still under development.
                     		
                  </help>
               </p>
            </li>
            <li>
               <h2><tt>name</tt></h2><code>--name  tag1,tag2 [tag1,tag2 ....]{1,*}</code><br><em>Methods: </em> parse: parseNames<br><br><em>  b: </em><code>-n</code><em>  c: </em><code>org.xmlcml.cmine.args.StringPair</code><p>
                  <h3>Description</h3>
                  <help>
                     			NAME:
                     			NOT YET IMPLEMENTED
                     			List of comma-separated pairs of tags; the first is replaced by the
                     			second. Example might be:
                     			em,i strong,b
                     			i.e. replace all &lt;em&gt;...&lt;/em&gt; by &lt;i&gt;...&lt;/i&gt;
                     		
                  </help>
               </p>
            </li>
            <li>
               <h2><tt>pubstyle</tt></h2><code>--pubstyle pub_code{1,1}</code><br><em>Methods: </em> parse: parsePubstyle<br><br><em>  b: </em><code>-p</code><em>  c: </em><code></code><p>
                  <h3>Description</h3>
                  <help>
                     			PUBSTYLE:
                     			Code or mnemomic to identifier the publisher or journal style.
                     			this is a list of journal/publisher styles so Norma knows how to
                     			interpret the
                     			input. At present only one argument is allowed. The pubstyle determines the
                     			format
                     			of the XML or HTML, the metadata, and soon how to parse the PDF. At
                     			present we'll use
                     			mnemonics such as 'bmc' or 'biomedcentral.com' or 'cellpress'.
                     
                     			To get a list of these use "+"--pubstyle"+" without arguments. 
                     			
                     <p class="note">
                        			under early
                        			development and note also that publisher styles change and can be transferred
                        			between publishers and journals
                        			
                     </p>
                     			
                     <p class="note">Does not trigger any actions</p>
                     		
                  </help>
               </p>
            </li>
            <li>
               <h2><tt>standalone</tt></h2><code>--standalone boolean{0,1}</code><br><em>Methods: </em> parse: parseStandalone<br><br><em>  b: </em><code></code><em>  c: </em><code>java.lang.Boolean</code><p>
                  <h3>Description</h3>
                  <help>
                     			STANDALONE:
                     			Treats XML document as standalone. Very useful as some parsers
                     			will take ages resolving the DTD and often fail
                     			if not connected to the net.
                     		
                  </help>
               </p>
            </li>
            <li>
               <h2><tt>strip</tt></h2><code>--strip [options to strip]{0,*}</code><br><em>Methods: </em> parse: parseStrip<br><br><em>  b: </em><code>-s</code><em>  c: </em><code></code><p>
                  <h3>Description</h3>
                  <help>
                     			List of XML components to strip from the raw well-formed HTML;
                     			
                     <ul>
                        				
                        <li>if a list is given, then use that; if this argument is missing
                           					(or
                           					the single string '#pubstyle' then the Pubstyle defaults are
                           					used.
                           				
                        </li>
                        				
                        <li>If there are no arguments then no stripping is done. a
                           					single '?'
                           					will list the Pubstyle defaults
                           				
                        </li>
                        			
                     </ul>
                     			The following are allowed:
                     			
                     <ul>
                        				
                        <li>
                           					an element local name (e.g.
                           					<tt>input</tt>
                           					)
                           				
                        </li>
                        				
                        <li>
                           					an XPath expression (e.g.
                           					<tt>//*[@class='foobar']</tt>
                           					)
                           				
                        </li>
                        				
                        <li>
                           					<tt>!DOCTYPE</tt>
                           					(strips
                           					<tt>&lt;!DOCTYPE ...&gt;</tt>
                           					which speeds up reading)
                           				
                        </li>
                        				
                        <li>an attribute (e.g. @class) (NotYetImplemented)</li>
                        			
                     </ul>
                     			
                     <p class="note">
                        				tokens are whitespace-separated (sorry if this interacts with
                        				XPath)
                        				Examples:
                        				
                        <ul>
                           					
                           <li>
                              						input script object (removes these three element
                              						<tt>//*[contains(@class,'sidebar')] (removes &lt;div
                                 							class='sidebar'&gt; ...
                                 							&lt;/div&gt;
                                 						</tt>
                              					
                           </li>
                           					
                           <li>!DOCTYPE (removes &lt;!DOCTYPE ...&gt; before parsing))</li>
                           					
                           <li>!DOCTYPE input script object //*[contains(@class,'sidebar')]
                              						(removes all the
                              						above)
                              					
                           </li>
                           				
                        </ul>
                        			
                     </p>
                     			
                     <p class="note">some of this is hardcoded in HTMLCleaner and should be refactored</p>
                     		
                  </help>
               </p>
            </li>
            <li>
               <h2><tt>tidy</tt></h2><code>--tidy [HTML tidy option]{1,1}</code><br><em>Methods: </em> parse: parseTidy<br><br><em>  b: </em><code>-t</code><em>  c: </em><code></code><p>
                  <h3>Description</h3>
                  <help>
                     			OBSOLETE - use --html
                     			TIDY:
                     			Choose an HTML tidy tool. At present we have:
                     			JTidy JSoup and HtmlUnit
                     			(NYI) This is very experimental at present.
                     		
                  </help>
               </p>
            </li>
            <li>
               <h2><tt>html</tt></h2><code>--html [HTML tidy option, jsoup, jtidy, htmlunit]{1,1}</code><br><em>Methods: </em> parse: parseHtml run: runHtml<br><p>
                  <h3>Description</h3>
                  <help>
                     			Clean raw HTML and produce XHTML. Requires heuristics:
                     			
                     <ul>
                        			
                        <li>quickscrape and getpapers create a JS DOM which has
                           			normalised some, but not all, elements
                        </li>
                        			
                        <li>remove &lt;script&gt; and other elements which are likely
                           			to be non-XML-compliant. Adjustable through &lt;value&gt;
                        </li>
                        			
                        <li>remove other artefacts (e.g. attributes like "=""")</li>
                        			
                        <li>run through html-tidy (jsoup, jtidy, htmlunit). They all try
                           			to sort out mess - the last is probably the best but I haven't
                           			bolted it in yet.
                        </li>
                        			
                        <li>remove empty &lt;div&gt;s, &lt;p&gt;s, etc.</li>
                        			
                     </ul>
                     			This is NOT scholarly.html and will need a publisher-specific
                     			stylesheet for further processing.
                     		
                  </help>
               </p>
            </li>
            <li>
               <h2><tt>xsl</tt></h2><code>--xsl stylesheet{1,5}</code><br><em>Methods: </em> parse: parseXsl run: transform<br><br><em>  b: </em><code>-x</code><em>  c: </em><code></code><p>
                  <h3>Description</h3>
                  <help>
                     			DEPRECATED (use transform)
                     		
                  </help>
               </p>
            </li>
            <li>
               <h2><tt>transform</tt></h2><code>--transform stylesheet{1,*}</code><br><em>Methods: </em> parse: parseTransform run: runTransform<br><p>
                  <h3>Description</h3>
                  <help>
                     		
                     <p class="brief">
                        			Transform XML or HTML or PDF or other input
                     </p>
                     			
                     <p class="note">Relacement for --xsl</p>
                     			
                     <p>Argument may be a file/URL reference to a
                        			stylesheet,
                        			or a code from one of {nlm, jats, pdfbox, hocr2svg, pdf2txt, jsoup,
                        			[jtidy, htmlunit NYI]}
                        			the codes are checked first and then the document reference.
                        
                        			Requires input and output files (--input and --output). These must be
                        			reserved names
                        			(e.g. fulltext.xml) in cmDir OR files in reserved directories and
                        			determine the type of files to convert.
                        			
                     </p>
                     		
                  </help>
               </p>
            </li>
            <li>
               <h2><tt>tag</tt></h2><code>--tag  {0,*}</code><br><code>
                  					Default:
                  					 src/main/resources/org/xmlcml/norma/pubstyle/sectionTagger.xml</code><em>Methods: </em> parse: parseTag<br><br><em>  b: </em><code></code><em>  c: </em><code>java.lang.String</code><p>
                  <h3>Description</h3>
                  <help>
                     
                     <p class="brief">Adds section tags to scholarly.html</p>
                     
                     <p>Not yet implemented</p>
                     
                  </help>
               </p>
            </li>
         </ul>
         <p>
            			NOTE:
            			
            <p class="note">
               			These examples contain some symbolic directory names.
               			
               <ul>
                  				
                  <li>
                     					<code>$ctree</code>
                     					represent the top of a ctree (e.g.
                     					<code>PMC12345</code>
                     					in
                     					<code>PMC12345/fulltext.xml</code>
                     					)
                     				
                  </li>
                  				
                  <li>
                     					<code>$parent</code>
                     					represents the parent of one or more <em><code>ctree</code></em>s, e.g.
                     					(e.g.
                     					<code>my/files/</code>
                     					in
                     					<code>my/files/PMC12345/fulltext.xml
                        					, my/files/PMC12349/fulltext.xml
                        				</code>
                     					)
                     				
                  </li>
                  				
                  <li>
                     					<code>*</code>
                     					is implicit and represents one or more <em><code>ctree</code></em>s, e.g.
                     					<code>my/files/*/fulltext.xml</code>
                     					in
                     					<code>my/files/PMC12345/fulltext.xml
                        					, my/files/PMC12349/fulltext.xml
                        				</code>
                     					)
                     				
                  </li>
                  				
                  <li>
                     					<code>wild*</code>
                     					is explicit and represents one or more <em><code>ctree</code></em>s, e.g.
                     					<code>my/files/PMC1234*/fulltext.xml</code>
                     					in
                     					<code>my/files/PMC12345/fulltext.xml
                        					, my/files/PMC12349/fulltext.xml
                        				</code>
                     					)
                     				
                  </li>
                  			
               </ul>
               		
            </p>
         </p>
         <h3>Examples</h3>
         <ul>
            <li>
               <p><span class="bold">Input: </span><code>$ctree/fulltext.xml</code>;    <span class="bold">Output: </span><code>$ctree/scholarly.html</code></p>
               <p><span class="desc"> Convert a single <em><code>ctree</code></em> containing NLM-XML to
                     				scholarly HTML
                     			</span></p>
               <p><code>norma -q $ctree -i fulltext.xml -o
                     				scholarly.html --transform
                     				nlm2html
                     			</code></p>
            </li>
            <li>
               <p><span class="bold">Input: </span><code>$parent/wildcard*/fulltext.xml</code>;    <span class="bold">Output: </span><code>$parent/wildcard*/scholarly.pdf.txt</code></p>
               <p><span class="desc"> Use wildcard to select many matching <em><code>ctree</code></em>s
                     				containing NLM-XML and convert to scholarly HTML
                     			</span></p>
               <p><code>norma -q ./plosone/PMC4382* -i fulltext.xml -o
                     				scholarly.html --transform nlm2html
                     			</code></p>
            </li>
            <li>
               <p><span class="bold">Input: </span><code>$parent/*/fulltext.xml</code>;    <span class="bold">Output: </span><code>$parent/*/scholarly.pdf.txt</code></p>
               <p><span class="desc"> Converting NLM-XML in multiple <em><code>ctree</code></em>s to scholarly HTML </span></p>
               <p><code>norma -q ./plosone/ -i fulltext.xml -o scholarly.html
                     				--transform nlm2html
                     			</code></p>
            </li>
            <li>
               <p><span class="bold">Input: </span><code>$ctree/fulltext.pdf</code>;    <span class="bold">Output: </span><code>$ctree/scholarly.pdf.txt</code></p>
               <p><span class="desc">
                     				Convert a single
                     				<em><code>ctree</code></em>
                     				containing
                     				<code>fulltext.pdf</code>
                     				to plain text
                     			</span></p>
               <p><code>norma -q ./peerjpdf/PMC4389270/ --transform pdf2txt -i
                     				fulltext.pdf
                     				-o fulltext.pdf.txt
                     			</code></p>
            </li>
            <li>
               <p><span class="bold">Input: </span><code>$parent/*/fulltext.pdf</code>;    <span class="bold">Output: </span><code>$parent/*/scholarly.pdf.txt</code></p>
               <p><span class="desc">
                     				Convert multiple <em><code>ctree</code></em>s (children of
                     				<code>$parent</code>
                     				) containing
                     				<code>fulltext.pdf</code>
                     				to
                     				plain text
                     			</span></p>
               <p><code>norma -q ./peerjpdf/ --transform pdf2txt -i fulltext.pdf -o
                     				fulltext.pdf.txt
                     			</code></p>
            </li>
            <li>
               <p><span class="bold">Input: </span><code>$ctree/fulltext.html</code>;    <span class="bold">Output: </span><code>$ctree/tidied.html</code></p>
               <p><span class="desc"> Tidy raw HTML in <code>$ctree</code> with <code>htmlunit</code> </span></p>
               <p><code>norma -q https_peerj.com_articles_384/ -i fulltext.html -o
                     				tidied.html --html htmlunit
                     			</code></p>
            </li>
            <li>
               <p><span class="bold">Input: </span><code>$ctree/fulltext.html</code>;    <span class="bold">Output: </span><code>$ctree/tidied.html</code></p>
               <p><span class="desc"> Tidy raw HTML in <code>$ctree</code> with <code>jtidy</code> </span></p>
               <p><code>norma -q https_peerj.com_articles_384/ -i fulltext.html -o
                     				tidied.html --html jtidy
                     			</code></p>
            </li>
            <li>
               <p><span class="bold">Input: </span><code>$ctree/fulltext.html</code>;    <span class="bold">Output: </span><code>$ctree/tidied.html</code></p>
               <p><span class="desc"> Tidy raw HTML in <code>$ctree</code> with <code>jsoup</code> </span></p>
               <p><code>norma -q https_peerj.com_articles_384/ -i fulltext.html -o
                     				tidied.html --html jsoup
                     			</code></p>
            </li>
            <li>
               <p><span class="bold">Input: </span><code>$parent/*/fulltext.html</code>;    <span class="bold">Output: </span><code>$parent/*/tidied.html</code></p>
               <p><span class="desc">
                     				tidy multiple child <em><code>ctree</code></em>s of
                     				<tt>$parent</tt>
                     				using
                     				<tt>jsoup</tt>
                     			</span></p>
               <p><code>norma -q $parent -i fulltext.html -o tidied.html --html
                     				jsoup</code></p>
            </li>
         </ul>
      </div>
   </body>
</html>