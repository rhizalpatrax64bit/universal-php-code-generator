<h2>UNIVERSAL PHP CODE GENERATOR</h2>

<p>As its name, UPCG aims to be a handy tool for developers to generate codes which have pattern. This means developers do not have to write repetitive codes. These codes are applied in application and in conjunction with a particular database and a particular PHP framework. UPCG does not strict itself to any database or any PHP framework. At present, UPCG can works with MySQL, SQL Server, and PostgreSQL. PHP frameworks we are talking about are not limited to MVC framework, but also CMF, CMS, and other terms which has a rule to integrate with it. After code generation, there is no single UPCG files included in the application. A simple example of UPCG generated code usage is CRUD application. You can modify these codes to create more complex application.</p>

<p>UPCG will read the database structure, make it available in the form of assigned variables, and assign these variables value to the templates. The templates are the responsibility of developer to determine it. There must be matching between variables provided by UPCG and variables needed to assign in templates.</p>

<p>UPCG designed to be agnostic to any templates. You can design template as complex as you can. You can even design templates to generate source code for languages other than PHP.<p>

<p>For some frameworks that have their own generator, UPCG act as an alternative which offers attractive features that are not present in those frameworks. As a <b>template centric</b> tool, UPCG gives developers a chance to write a clean and compact template. Here are UPCG features and usage practice:</p>

<ol>
<li>Unlike the others, there is no new things in UPCG, everything is still PHP. You do not have to write new syntax that can confuse you. UPCG heavily exploits short open tag syntax to obtain variable value, but you do not have to swich this directive on. By default, UPCG will convert this tag to long open tag, and you can evaluate this transformation for debugging. UPCG writes every modifications of templates to files,</li>

<li>You are encouraged to write 'if statement' that will echo someting in a compact form (<font face='courier'><?=($val1==$val2)? $val3 : $val4?></font>), in order to mimic simple variable echo (<font face='courier'><?=$val?></font>).</li>

<li>To obtain readable code that is embedded in the template, sometimes You write logic syntax in seperate line and give extra space before PHP open tag. This practise will produce extra space that make a mess code inlining in generated files. Fortunately, by default UPCG will remove this extra space before parses the template.</li>

<li>You are encouraged to write 'if/for/foreach/while statement' in bracketless form.</li>

<li>You are encouraged to do 'one set enclosed tag per statement', 'as few logic statement as possible', and 'remove semicolon close to the closing tag' practice.<br>
<br>
<pre>
example of:<br>
- point 2: line 6<br>
- point 3: line 4, 5, 7, 9, 10, 13<br>
- point 4: line 4, 7, 9, 10<br>
- point 5: line 5<br>
<br>
01 |this line does not have inline space<br>
02 |    this is predecessor line that have inline space.<br>
03 |    UPCG can handle conditional/loop statement bellow to get same inline level with these line.<br>
04 |    <?foreach($var as $item):><br>
05 |        <?$i++?><br>
06 |    string contain iterateable <?$item?> outside php tag, but inside the loop. <?=($i==3)? $smiley : $sad?><br>
07 |        <?if($item == $something):?><br>
08 |    this string still have same inline level regardless of additional space of IF.<br>
09 |        <?endif?><br>
10 |    <?endforeach?><br>
11 |<br>
12 |    UPGC also can handle multiline logic statements, as long as there is only PHP open tag in the line.<br>
13 |    <?<br>
14 |        // logic statements span in multiline<br>
15 |    ?><br>
16 |    additional line that have same inline level.<br>
17 |this last line does not have inline space.<br>
<br>
modified template produced by UPCG:<br>
<br>
01 |this line does not have inline space<br>
02 |    this is predecessor line that have inline space.<br>
03 |    UPCG can handle conditional/loop statement bellow to get same inline level with these line.<br>
04 |<?foreach($var as $item):><br>
05 |<?$i++?><br>
06 |    string contain iterateable <?$item?> outside php tag, but inside the loop. <?=($i==3)? $smiley : $sad?><br>
07 |<?if($item == $something):?><br>
08 |    this string still have same inline level regardless of additional space of IF.<br>
09 |<?endif?><br>
10 |<?endforeach?><br>
11 |<br>
12 |    UPGC also can handle multiline logic statements, as long as there is only PHP open tag in the line.<br>
13 |<?<br>
14 |        // logic statements span in multiline<br>
15 |    ?><br>
16 |    additional line that have same inline level.<br>
17 |this last line does not have inline space.<br>
</pre>

</li>

<li>UPCG can handle variable echo followed by a closing tag at the end of line, without additional echo of new line character. Otherwise, php will merge that line with line immediately bellow it.<br>
<br>
<pre>
in ordinary php code, you have to do <?=$likeThis."\n"?><br>
in order to prevent merger between the above line with this line.<br>
<br>
in UPCG template, you are save to <?=$doThis?><br>
UPCG recognize this form, and automatically prevents merger.<br>
</pre>

</li>

<li>The more words/letters that you write in a line, the longer time you need to understand it. In the templates, You need to access every properties and methods available in the UPCG class by <font face='courier'>'$myObject->...'</font>. Fortunately, UPCG provides shortcuts so you can immediately write property like this: <font face='courier'>'$propertyname'</font>, and method like this: <font face='courier'>'methodname($param)'</font>, in order to mimic global variables and functions. This way, you still can access other properties and methods in the class. You can write your own shortcut (for Your custom methods only) by a simple rule. For Your custom properties, you even do not have to do anything. UPCG automatically do this for you.<br>
<br>
<pre>
in ordinary php code, you have to write:<br>
$this->set<?=$myObject->getCapital($myObject->column)?>($primary_key['<?=$myObject->foreignColumn?>']);<br>
<br>
in UPCG template, you can write:<br>
$this->set<?=getCapital($column)?>($primary_key['<?=$foreignColumn?>']);<br>
<br>
the example output is:<br>
$this->setMyTable($primary_key['my_table_id']);<br>
</pre>

</li>

<li>In the IF statement, sometimes we have to include extra blank line inside the statement in order to seperate the output of IF from the surrounding lines. Sometimes this practice force us to stick one tag of IF statement (weather it is the open IF tag <code>[</code>if(...)], the close tag <code>[</code>endif], or the close bracket <code>[</code>}]) to other line, and we are confused about the logic because of that. Fortunately, by default UPCG rearrange the IF close tag so we can write both tag tightly to the content of IF, and place extra blank line outside the IF without worry about surplus blank line in the output.<br>
<br>
<pre>
this is output we want to get:<br>
<br>
01 |text before IF STATEMENT<br>
02 |<br>
03 |text inside IF STATEMENT<br>
04 |<br>
05 |text after IF STATEMENT<br>
<br>
in ordinary PHP code, we have to write like this:<br>
<br>
01 |text before IF STATEMENT<br>
02 |<br>
03 |<?if($var1==$var2)?><br>
04 |text inside IF STATEMENT<br>
05 |<br>
06 |<?endif?><br>
07 |text after IF STATEMENT<br>
<br>
in UPCG template, you are save to write like this:<br>
<br>
01 |text before IF STATEMENT<br>
02 |<br>
03 |<?if($var1==$var2)?><br>
04 |text inside IF STATEMENT<br>
05 |<?endif?><br>
06 |<br>
07 |text after IF STATEMENT<br>
</pre>

You can alter behaviour of point 1,3,6,7,8 if you need/want to write template that is not as recommended.<br /><br /></li>

<li>every configuration in UPCG is centralized and hierarchical. If you have a lot of templates for generated, and only a few templates that do not follow the standard behaviour, you do not have to run the generator several times. You only need to configure properly.<br>
<br>
<pre>
/*global setting*/<br>
'removeMessySpaceInTemplate' => true,<br>
'rearrangeClosingTagInTemplate' => true,<br>
<br>
/*for 'model' only*/<br>
'removeMessySpaceInTemplate' => false,<br>
'rearrangeClosingTagInTemplate' => false,<br>
<br>
/*for 'controller' only*/<br>
'removeMessySpaceInTemplate' => true,<br>
'rearrangeClosingTagInTemplate' => false,<br>
</pre>

</li>

<li>Variables and methods provided by UPCG only the essential. Because everyone's needs are different, UPCG provide facilities to expand the generator. First, You can add some variables in the configuration. In templates, You can incorporate these variables in conditional or loop you have design, for a deeper customization. Same as point-8, every custom variables can be designed hierarchical. Second, If that way does not enought, You can extend UPCG class. Finally, You can control your custom method behaviour in the configuration, in order to get specialized templates processing.<br>
<br>
<pre>
/*custom var in global*/<br>
'myCustomVar1' => 'asdf',<br>
'myCustomVar2' => 'qwer',<br>
<br>
/*custom var for 'model' only*/<br>
'MyCustomVar1' => 'zxcv',        // override global<br>
'MyCustomVar3' => 'uiop',<br>
</pre>

</li>

<li>UPCG tries to acomodate some popular frameworks like CakePHP which has special need for inflection. UPCG provides this feature, so CakePHP users can work the same way as they work with their framework. As a note, this feature has not been finished.</li>

</ol>

<p>You will appreciate this design after you have tried. Now You can focus on template design. Please take a look at templates and configuration files.</p>


<h3>REQUIREMENT</h3>
<ul>
<li>PHP 5.3</li>
<li>database access</li>
<li>file system write access</li>
</ul>


<h3>USER MANUAL</h3>

<p>There are pregenerated files in 'sample1/output' folder. These files are based on templates located in 'sample1/templates' folder and 'sample1/db/test.sql' schema, You can learn from its.</p>

<h4>TO GENERATE FILES</h4>

<ol>
<li>Make your own templates or modify existing templates.</li>

<li>Adjust all configuration related to 'database', 'templates' and/or 'output' in <font face='courier'>$config</font> array in 'upcg-run.php' file. The <font face='courier'>$config</font> is self-explanatory. Change as few as possible.</li>

<li>The output files in <font face='courier'>$config<code>[</code>'GENERATIONTYPE']<code>[</code>'FILETYPE']</font> have a naming pattern. You can use any non-array/object variables in <font face='courier'>$config</font> or class property as a pattern (see next section for conplete list of class properties). Patterns are applied not only to file name, but also to folder. Word must be enclosed with curly brackets to be recognized as a variable. There is no limitation on how many variable You use. You do not have to provide directory, UPCG automatically create if it does not exist.<br>
<br>
<pre>
'outputFile' => 'controllers/{className}Controller.php',<br>
'outputFile' => 'views/scripts/{className}/edit.phtml',<br>
</pre>

</li>

<li>If You incorporate additional templates, add in configuration related to 'file types'. You can add as much as You need.</li>

<li>If You need additional variables that accessible in the template, add in configuration related to 'global custom var' (<font face='courier'>$config<code>[</code>'globalCustomVar']</font>) or 'file type custom var' (<font face='courier'>$config<code>[</code>'GENERATIONTYPE']<code>[</code>'FILETYPE']<code>[</code>'filetypeCustomVar']</font>). Use 'global custom var' as a priority place.</li>

<li>If You need additional methods in the templates, write your own class and extend the '<font face='courier'>UPCG_ZDMG</font>' or '<font face='courier'>UPCG_Cakephp13</font>' class. Adjust configuration related to 'generator class'. You can create shortcuts for those methods in configuration related to 'template'.<br />note: '<font face='courier'>UPCG_Cakephp13</font>' class is not finished yet. All other class in the 'libs' folder are not intended to be derived. They are not accessible to templates.</li>

<li>Execute  the 'upcg-run.php' in command prompt.<br>
<pre>
$ php upcg-run.php<br>
</pre>

</li>

<li>If the process runs correctly, the screen will display '<font face='courier'>done!</font>'.</li>
</ol>

<h4>TO DESIGN TEMPLATE:</h4>
<ul>
<li>All items in the 'global custom var' and 'file type custom var' are available to the template. If you need to access variable already defined outside the two, you have to re-declare it in one of them.</li>

<li>Template design is tightly bind to the generator class that You use. You tell UPCG to use generator in '<font face='courier'>generatorFile</font>' and '<font face='courier'>generatorClassName</font>' items. If You do not give values to them, UPCG will give you standard generator class.</li>

<li>There is no relevant method available to the templates in standard generator class.</li>

<li>The relevant standard generator class properties available to the templates are (in structured form):<br>
<pre>
[tableName] => string<br>
<br>
[columns] => compound array<br>
[0]   [field] => string<br>
[type] => string<br>
[phptype] => string<br>
[comment] => string<br>
[...] [...]<br>
<br>
[className] => string<br>
<br>
[classDesc] => string<br>
<br>
[primaryKey] => simple array<br>
[field] => string<br>
[type] => string<br>
[phptype] => string<br>
[foreignKey] => string<br>
<br>
or:<br>
<br>
[primaryKey] => compound array<br>
[field]('string', ...)<br>
[type] => null<br>
[phptype] => null<br>
[fields] => compound array<br>
[0]   [field] => string<br>
[type] => string<br>
[phptype] => string<br>
[foreignKey] => string<br>
[...] [...]<br>
[foreignKey] => null<br>
<br>
[foreignKeys] null or compound array<br>
[0]   [keyName] => string<br>
[columnName] => string<br>
[foreignTableName] => string<br>
[foreignTableColumnName] => string<br>
[...] [...]<br>
<br>
[dependentTables] => null or compound array<br>
[0]   [keyName] => string<br>
[type] => many<br>
[columnName] => string<br>
[foreignTableName] => string<br>
[foreignTableColumnName] => string<br>
[...] [...]<br>
</pre>

</li>

<li>If You use <font face='courier'>UPCG_ZDMG</font> class, the relevant methods available to the templates are:<br>
<blockquote><ul>
<blockquote><li>getCapital: simply convert string from '<font face='courier'>ab_cd_ef</font>' to '<font face='courier'>AbCdEf</font>'</li>
<li>getClassName: simply convert string from '<font face='courier'>tbl_ab_cd_ef_id</font>' to '<font face='courier'>AbCdEf</font>'.</li>
<li>getRelationName: Convert string from '<font face='courier'>ab_cd_ef</font>' to '<font face='courier'>AbCdEf</font>' and append some words if a conflict occurs. You can learn from pregenerated code to get deeper understanding.</li>
</blockquote></ul>
</li></blockquote>

<li>There are additional variables available to the templates in <font face='courier'>UPCG_ZDMG</font> class:<br>
<pre>
[columns][...][capital] => string<br>
[primaryKey][capital] => string<br>
or<br>
[primaryKey][fields][...][capital] => string<br>
</pre>

</li>

<li>You can call global functions in Template.</li>

<li>Follow the guidence on the 'UPCG features and usage practice' list (point 1-8).</li>

<li>Use custom variables as few as possible. If a string appears in many places, and have to be changed frequently, then it deserves to be a variable.</li>
</ul>

<h3>ACKNOWLEDGMENT</h3>
<ul>
<li>UPCG is based on <a href='http://code.google.com/p/zend-db-model-generator/'>'Zend Db Model Generator' project</a>. This tool only generates three types of files: DbTable, Mapper, and Model, and tightly bind with Zend framework. This project is GNU LGPL licenced. Now UPCG do not longer reflect this project. There is only a little similarities between UPCG and ZDMG: a basic idea on how to generate files.</li>

<li>UPCG incorporate 'Inflector.php' file from CakePHP, in order to be able to generate files with the same pattern. This file is MIT licenced.</li>

<li>Notepad++ is used as editor. It is excellent for PHP syntax highlighting.</li>

<li>WinMerge is used for comparing the original output of ZDMG and output of UPCG.</li>

<li>MySQL workbench is used to create example schema.</li>
</ul>

<h3>DEDICATION</h3>
<p>Anisa Fahmi, K. A. Fathurroyyan, and our big family.</p>


<h3>CONTACT</h3>
<p>If You have any comments / suggestions / Questions or want to contribute, please send email to: <a href='mailto://the.liquid.metal@gmail.com'>the.liquid.metal@gmail.com</a> (Hendra Gunawan)</p>
