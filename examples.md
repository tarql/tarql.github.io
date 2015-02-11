---
layout: page
title: Examples
permalink: /examples/
---

### Example \#1

#### CSV data

The following lines are part of a CSV file ([TechCrunchcontinentalUSA.csv](https://github.com/emir-munoz/tarql/tree/master/examples/TechCrunch/TechCrunchcontinentalUSA.csv)) 
snapshot that contains information about companies.

{% highlight bash %}
permalink,company,numEmps,category,city,state,fundedDate,raisedAmt,raisedCurrency,round
lifelock,LifeLock,,web,Tempe,AZ,1-May-07,6850000,USD,http://example.com/b
lifelock,LifeLock,,web,Tempe,AZ,1-Oct-06,6000000,USD,<http://example.com/a>
lifelock,LifeLock,,web,Tempe,AZ,1-Jan-08,25000000,USD,c
mycityfaces,MyCityFaces,7,web,Scottsdale,AZ,1-Jan-08,50000,USD,seed
flypaper,Flypaper,,web,Phoenix,AZ,1-Feb-08,3000000,USD,a
infusionsoft,Infusionsoft,105,software,Gilbert,AZ,1-Oct-07,9000000,USD,a
gauto,gAuto,4,web,Scottsdale,AZ,1-Jan-08,250000,USD,seed
chosenlist-com,ChosenList.com,5,web,Scottsdale,AZ,1-Oct-06,140000,USD,seed
chosenlist-com,ChosenList.com,5,web,Scottsdale,AZ,25-Jan-08,233750,USD,angel
digg,Digg,60,web,San Francisco,CA,1-Dec-06,8500000,USD,b
digg,Digg,60,web,San Francisco,CA,1-Oct-05,2800000,USD,a
facebook,Facebook,450,web,Palo Alto,CA,1-Sep-04,500000,USD,angel
facebook,Facebook,450,web,Palo Alto,CA,1-May-05,12700000,USD,a
facebook,Facebook,450,web,Palo Alto,CA,1-Apr-06,27500000,USD,b
facebook,Facebook,450,web,Palo Alto,CA,1-Oct-07,300000000,USD,c
facebook,Facebook,450,web,Palo Alto,CA,1-Mar-08,40000000,USD,c
facebook,Facebook,450,web,Palo Alto,CA,15-Jan-08,15000000,USD,c
facebook,Facebook,450,web,Palo Alto,CA,1-May-08,100000000,USD,debt_round
...
{% endhighlight %}

#### Tarql mapping

We could define a mapping to convert the previous CSV file into RDF triples as follows:

{% highlight bash %}
PREFIX ex: <http://ex.org/a#>
PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>

CONSTRUCT {
  ?URI a ex:Organization;
    ex:permalink ?permalink;
    ex:name ?company;
    ex:employees ?numEmployees;
    ex:category ?category;
    ex:city ?city;
    ex:state ?state;
    ex:fundationDate ?fundedDate;
    ex:raisedAmt ?amount;
    ex:raisedCurrency ?raisedCurrency;
    ex:round ?round;
} 
FROM <file:TechCrunchcontinentalUSA.csv> 
WHERE {
  BIND (URI(CONCAT('http://ex.org/companies/', ?permalink)) AS ?URI)
  BIND (xsd:integer(?numEmps) AS ?numEmployees)
  BIND (xsd:decimal(?raisedAmt) AS ?amount)
}
{% endhighlight %}

In the previous mapping we have indicated the following:

* how each column will be mapped with a predicate, with respect to the main resource (?URI)
* which file are we trying to convert (`file:TechCrunchcontinentalUSA.csv`)
* how we can apply datatypes to some of the variables (BIND clauses)

#### Result

Executing Tarql with the previous mapping:

{% highlight bash %}
~$ cd tarql/target/appassembler
~$ sh bin/tarql --ntriples ../../examples/TechCrunch/sample-2.sparql ../../examples/TechCrunch/TechCrunchcontinentalUSA.csv
{% endhighlight %}

We do get the following RDF in N-Triples format.

{% highlight bash %}
<http://ex.org/companies/lifelock> <http://www.w3.org/1999/02/22-rdf-syntax-ns#type> <http://ex.org/a#Organization> .
<http://ex.org/companies/lifelock> <http://ex.org/a#permalink> "lifelock" .
<http://ex.org/companies/lifelock> <http://ex.org/a#name> "LifeLock" .
<http://ex.org/companies/lifelock> <http://ex.org/a#category> "web" .
<http://ex.org/companies/lifelock> <http://ex.org/a#city> "Tempe" .
<http://ex.org/companies/lifelock> <http://ex.org/a#state> "AZ" .
<http://ex.org/companies/lifelock> <http://ex.org/a#fundationDate> "1-May-07" .
<http://ex.org/companies/lifelock> <http://ex.org/a#raisedAmt> "6850000"^^<http://www.w3.org/2001/XMLSchema#decimal> .
<http://ex.org/companies/lifelock> <http://ex.org/a#raisedCurrency> "USD" .
<http://ex.org/companies/lifelock> <http://ex.org/a#round> <http://example.com/b> .
<http://ex.org/companies/lifelock> <http://www.w3.org/1999/02/22-rdf-syntax-ns#type> <http://ex.org/a#Organization> .
<http://ex.org/companies/lifelock> <http://ex.org/a#permalink> "lifelock" .
<http://ex.org/companies/lifelock> <http://ex.org/a#name> "LifeLock" .
<http://ex.org/companies/lifelock> <http://ex.org/a#category> "web" .
<http://ex.org/companies/lifelock> <http://ex.org/a#city> "Tempe" .
<http://ex.org/companies/lifelock> <http://ex.org/a#state> "AZ" .
<http://ex.org/companies/lifelock> <http://ex.org/a#fundationDate> "1-Oct-06" .
<http://ex.org/companies/lifelock> <http://ex.org/a#raisedAmt> "6000000"^^<http://www.w3.org/2001/XMLSchema#decimal> .
<http://ex.org/companies/lifelock> <http://ex.org/a#raisedCurrency> "USD" .
<http://ex.org/companies/lifelock> <http://ex.org/a#round> <http://example.com/> .
<http://ex.org/companies/lifelock> <http://www.w3.org/1999/02/22-rdf-syntax-ns#type> <http://ex.org/a#Organization> .
<http://ex.org/companies/lifelock> <http://ex.org/a#permalink> "lifelock" .
<http://ex.org/companies/lifelock> <http://ex.org/a#name> "LifeLock" .
<http://ex.org/companies/lifelock> <http://ex.org/a#category> "web" .
<http://ex.org/companies/lifelock> <http://ex.org/a#city> "Tempe" .
<http://ex.org/companies/lifelock> <http://ex.org/a#state> "AZ" .
<http://ex.org/companies/lifelock> <http://ex.org/a#fundationDate> "1-Jan-08" .
<http://ex.org/companies/lifelock> <http://ex.org/a#raisedAmt> "25000000"^^<http://www.w3.org/2001/XMLSchema#decimal> .
<http://ex.org/companies/lifelock> <http://ex.org/a#raisedCurrency> "USD" .
<http://ex.org/companies/lifelock> <http://ex.org/a#round> "c" .
<http://ex.org/companies/mycityfaces> <http://www.w3.org/1999/02/22-rdf-syntax-ns#type> <http://ex.org/a#Organization> .
<http://ex.org/companies/mycityfaces> <http://ex.org/a#permalink> "mycityfaces" .
<http://ex.org/companies/mycityfaces> <http://ex.org/a#name> "MyCityFaces" .
<http://ex.org/companies/mycityfaces> <http://ex.org/a#employees> "7"^^<http://www.w3.org/2001/XMLSchema#integer> .
<http://ex.org/companies/mycityfaces> <http://ex.org/a#category> "web" .
<http://ex.org/companies/mycityfaces> <http://ex.org/a#city> "Scottsdale" .
<http://ex.org/companies/mycityfaces> <http://ex.org/a#state> "AZ" .
<http://ex.org/companies/mycityfaces> <http://ex.org/a#fundationDate> "1-Jan-08" .
<http://ex.org/companies/mycityfaces> <http://ex.org/a#raisedAmt> "50000"^^<http://www.w3.org/2001/XMLSchema#decimal> .
<http://ex.org/companies/mycityfaces> <http://ex.org/a#raisedCurrency> "USD" .
<http://ex.org/companies/mycityfaces> <http://ex.org/a#round> "seed" .
...
{% endhighlight %}

<hr>

### Example \#2

#### CSV data

The following lines are part of a CSV file ([table_2.csv](https://github.com/emir-munoz/tarql/blob/master/examples/ArsenalFC/table_2.csv)) 
snapshot that contains information first-team squad of Arsenal F.C. according to its Wikipedia [article](https://en.wikipedia.org/wiki/Arsenal_F.C.).

{% highlight bash %}
"No.","","Position","Player"
"1","http://en.wikipedia.org/wiki/Poland","http://en.wikipedia.org/wiki/Goalkeeper_(association_football)","http://en.wikipedia.org/wiki/Wojciech_Szcz%C4%99sny"
"2","http://en.wikipedia.org/wiki/France","http://en.wikipedia.org/wiki/Defender_(association_football)","http://en.wikipedia.org/wiki/Mathieu_Debuchy"
"3","http://en.wikipedia.org/wiki/England","http://en.wikipedia.org/wiki/Defender_(association_football)","http://en.wikipedia.org/wiki/Kieran_Gibbs"
"4","http://en.wikipedia.org/wiki/Germany","http://en.wikipedia.org/wiki/Defender_(association_football)","http://en.wikipedia.org/wiki/Per_Mertesacker"
"6","http://en.wikipedia.org/wiki/France","http://en.wikipedia.org/wiki/Defender_(association_football)","http://en.wikipedia.org/wiki/Laurent_Koscielny"
"7","http://en.wikipedia.org/wiki/Czech_Republic","http://en.wikipedia.org/wiki/Midfielder","http://en.wikipedia.org/wiki/Tom%C3%A1%C5%A1_Rosick%C3%BD"
"8","http://en.wikipedia.org/wiki/Spain","http://en.wikipedia.org/wiki/Midfielder","http://en.wikipedia.org/wiki/Mikel_Arteta"
"9","http://en.wikipedia.org/wiki/Germany","http://en.wikipedia.org/wiki/Forward_(association_football)","http://en.wikipedia.org/wiki/Lukas_Podolski"
"10","http://en.wikipedia.org/wiki/England","http://en.wikipedia.org/wiki/Midfielder","http://en.wikipedia.org/wiki/Jack_Wilshere"
"11","http://en.wikipedia.org/wiki/Germany","http://en.wikipedia.org/wiki/Midfielder","http://en.wikipedia.org/wiki/Mesut_%C3%96zil"
"12","http://en.wikipedia.org/wiki/France","http://en.wikipedia.org/wiki/Forward_(association_football)","http://en.wikipedia.org/wiki/Olivier_Giroud"
"13","http://en.wikipedia.org/wiki/Colombia","http://en.wikipedia.org/wiki/Goalkeeper_(association_football)","http://en.wikipedia.org/wiki/David_Ospina"
"14","http://en.wikipedia.org/wiki/England","http://en.wikipedia.org/wiki/Forward_(association_football)","http://en.wikipedia.org/wiki/Theo_Walcott"
"15","http://en.wikipedia.org/wiki/England","http://en.wikipedia.org/wiki/Midfielder","http://en.wikipedia.org/wiki/Alex_Oxlade-Chamberlain"
"16","http://en.wikipedia.org/wiki/Wales","http://en.wikipedia.org/wiki/Midfielder","http://en.wikipedia.org/wiki/Aaron_Ramsey"
{% endhighlight %}

#### Tarql mapping

We could define a mapping to convert the previous CSV file into RDF triples as follows:

{% highlight bash %}
PREFIX dbp: <http://dbpedia.org/property/>
PREFIX dbo: <http://dbpedia.org/ontology/>
PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>

CONSTRUCT {
  ?Player a dbo:SoccerPlayer;
    dbp:number ?number;
    dbp:birthPlace ?b;
    dbp:position ?Position;
}
FROM <file:table_2.csv> 
WHERE {
  BIND (xsd:integer(?No) AS ?number)
}
{% endhighlight %}

In the previous mapping we have indicated the following:

* the last column is the main entity in each row which is a `dbo:SoccerPlayer`
* the second column doesn't contain a column name, so name `?b` is assigned by default
* which file are we trying to convert (`file:table_2.csv`)
* `?No` is the number of the player in the team which is an integer

#### Result

Executing Tarql with the previous mapping:

{% highlight bash %}
~$ cd tarql/target/appassembler
~$ sh bin/tarql --ntriples ../../examples/ArsenalFC/sample-arsenal-table_2.sparql ../../examples/ArsenalFC/table_2.csv
{% endhighlight %}

We do get the following RDF in N-Triples format.

{% highlight bash %}
<http://en.wikipedia.org/wiki/Wojciech_Szcz%C4%99sny> <http://www.w3.org/1999/02/22-rdf-syntax-ns#type> <http://dbpedia.org/ontology/SoccerPlayer> .
<http://en.wikipedia.org/wiki/Wojciech_Szcz%C4%99sny> <http://dbpedia.org/property/number> "1"^^<http://www.w3.org/2001/XMLSchema#integer> .
<http://en.wikipedia.org/wiki/Wojciech_Szcz%C4%99sny> <http://dbpedia.org/property/birthPlace> <http://en.wikipedia.org/wiki/Poland> .
<http://en.wikipedia.org/wiki/Wojciech_Szcz%C4%99sny> <http://dbpedia.org/property/position> <http://en.wikipedia.org/wiki/Goalkeeper_(association_football)> .
<http://en.wikipedia.org/wiki/Mathieu_Debuchy> <http://www.w3.org/1999/02/22-rdf-syntax-ns#type> <http://dbpedia.org/ontology/SoccerPlayer> .
<http://en.wikipedia.org/wiki/Mathieu_Debuchy> <http://dbpedia.org/property/number> "2"^^<http://www.w3.org/2001/XMLSchema#integer> .
<http://en.wikipedia.org/wiki/Mathieu_Debuchy> <http://dbpedia.org/property/birthPlace> <http://en.wikipedia.org/wiki/France> .
<http://en.wikipedia.org/wiki/Mathieu_Debuchy> <http://dbpedia.org/property/position> <http://en.wikipedia.org/wiki/Defender_(association_football)> .
<http://en.wikipedia.org/wiki/Kieran_Gibbs> <http://www.w3.org/1999/02/22-rdf-syntax-ns#type> <http://dbpedia.org/ontology/SoccerPlayer> .
<http://en.wikipedia.org/wiki/Kieran_Gibbs> <http://dbpedia.org/property/number> "3"^^<http://www.w3.org/2001/XMLSchema#integer> .
<http://en.wikipedia.org/wiki/Kieran_Gibbs> <http://dbpedia.org/property/birthPlace> <http://en.wikipedia.org/wiki/England> .
<http://en.wikipedia.org/wiki/Kieran_Gibbs> <http://dbpedia.org/property/position> <http://en.wikipedia.org/wiki/Defender_(association_football)> .
<http://en.wikipedia.org/wiki/Per_Mertesacker> <http://www.w3.org/1999/02/22-rdf-syntax-ns#type> <http://dbpedia.org/ontology/SoccerPlayer> .
<http://en.wikipedia.org/wiki/Per_Mertesacker> <http://dbpedia.org/property/number> "4"^^<http://www.w3.org/2001/XMLSchema#integer> .
<http://en.wikipedia.org/wiki/Per_Mertesacker> <http://dbpedia.org/property/birthPlace> <http://en.wikipedia.org/wiki/Germany> .
<http://en.wikipedia.org/wiki/Per_Mertesacker> <http://dbpedia.org/property/position> <http://en.wikipedia.org/wiki/Defender_(association_football)> .
<http://en.wikipedia.org/wiki/Laurent_Koscielny> <http://www.w3.org/1999/02/22-rdf-syntax-ns#type> <http://dbpedia.org/ontology/SoccerPlayer> .
<http://en.wikipedia.org/wiki/Laurent_Koscielny> <http://dbpedia.org/property/number> "6"^^<http://www.w3.org/2001/XMLSchema#integer> .
<http://en.wikipedia.org/wiki/Laurent_Koscielny> <http://dbpedia.org/property/birthPlace> <http://en.wikipedia.org/wiki/France> .
<http://en.wikipedia.org/wiki/Laurent_Koscielny> <http://dbpedia.org/property/position> <http://en.wikipedia.org/wiki/Defender_(association_football)> .
<http://en.wikipedia.org/wiki/Tom%C3%A1%C5%A1_Rosick%C3%BD> <http://www.w3.org/1999/02/22-rdf-syntax-ns#type> <http://dbpedia.org/ontology/SoccerPlayer> .
<http://en.wikipedia.org/wiki/Tom%C3%A1%C5%A1_Rosick%C3%BD> <http://dbpedia.org/property/number> "7"^^<http://www.w3.org/2001/XMLSchema#integer> .
<http://en.wikipedia.org/wiki/Tom%C3%A1%C5%A1_Rosick%C3%BD> <http://dbpedia.org/property/birthPlace> <http://en.wikipedia.org/wiki/Czech_Republic> .
<http://en.wikipedia.org/wiki/Tom%C3%A1%C5%A1_Rosick%C3%BD> <http://dbpedia.org/property/position> <http://en.wikipedia.org/wiki/Midfielder> .
...
{% endhighlight %}

### Example \#3

#### CSV data

The following lines are part of a CSV file ([table_1.csv](https://github.com/emir-munoz/tarql/blob/master/examples/Movies/table_1.csv)) 
snapshot that contains information extracted from the [article](http://www.the-numbers.com/movie/budgets/all).

{% highlight bash %}
" ","Release Date","Movie","Production Budget","Domestic Gross","Worldwide Gross"
"1","12/18/2009","Avatar","$425,000,000","$760,507,625","$2,783,918,982"
"2","5/24/2007","Pirates of the Caribbean: At World's End","$300,000,000","$309,420,425","$960,996,492"
"3","7/20/2012","The Dark Knight Rises","$275,000,000","$448,139,099","$1,079,343,943"
"4","7/2/2013","The Lone Ranger","$275,000,000","$89,289,910","$259,989,910"
"5","3/9/2012","John Carter","$275,000,000","$73,058,679","$282,778,100"
"6","11/24/2010","Tangled","$260,000,000","$200,821,936","$586,581,936"
"7","5/4/2007","Spider-Man 3","$258,000,000","$336,530,303","$890,875,303"
"8","12/14/2012","The Hobbit: An Unexpected Journey","$250,000,000","$303,003,568","$1,014,703,568"
"9","7/15/2009","Harry Potter and the Half-Blood Prince","$250,000,000","$301,959,197","$935,083,686"
"10","12/13/2013","The Hobbit: The Desolation of Smaug","$250,000,000","$258,366,855","$950,466,855"
"11","12/10/2014","The Hobbit: The Battle of the Five Armies","$250,000,000","$252,912,902","$917,112,902"
"12","5/20/2011","Pirates of the Caribbean: On Stranger Tides","$250,000,000","$241,063,875","$1,043,663,875"
"13","6/28/2006","Superman Returns","$232,000,000","$200,120,000","$374,085,065"
"14","11/14/2008","Quantum of Solace","$230,000,000","$169,368,427","$591,692,078"
"15","5/4/2012","The Avengers","$225,000,000","$623,279,547","$1,514,279,547"
"16","7/7/2006","Pirates of the Caribbean: Dead Man's Chest","$225,000,000","$423,315,812","$1,060,615,812"
{% endhighlight %}

#### Tarql mapping

We could define a mapping to convert the previous CSV file into RDF triples as follows:

{% highlight bash %}
PREFIX dbp: <http://dbpedia.org/property/>
PREFIX dbo: <http://dbpedia.org/ontology/>
PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX tarql: <http://tarql.github.io/tarql#>

CONSTRUCT {
  ?MovieUrl a dbo:Film;
    dbp:number ?id;
    dbp:released ?ReleaseDate;
    rdfs:label ?Movie;
    dbp:budget ?ProductionBudget;
    dbp:grossDomestic ?DomesticGross;
    dbp:grossWorldwide ?WorldwideGross;
}
FROM <file:table_1.csv> 
WHERE {
  BIND (URI(CONCAT('http://example.com/ns#', ?a)) AS ?MovieUrl)
  BIND (xsd:integer(?a) AS ?id)
  BIND (tarql:date(?Release_Date, 'MM/dd/yyyy') AS ?ReleaseDate)
  BIND (tarql:currency(?Production_Budget, 'USD') AS ?ProductionBudget)
  BIND (tarql:currency(?Domestic_Gross, 'USD') AS ?DomesticGross)
  BIND (tarql:currency(?Worldwide_Gross, 'USD') AS ?WorldwideGross)
}
{% endhighlight %}

In the previous mapping we have indicated the following:

* which file are we trying to convert (`file:table_1.csv`)
* the first column in the CSV is an identifier of the row, that can be used in the URI
* the first column doesn't contain a column name, so name `?a` is assigned by default
* `?a` is converted into an integer
* the second column is a date, so can be converted into `dateTime` datatype
* last three columns are money amounts that can be converted into the corresponding currency, USD for this case

#### Result

Executing Tarql with the previous mapping:

{% highlight bash %}
~$ cd tarql/target/appassembler
~$ sh bin/tarql --ntriples ../../examples/Movies/sample-movies.sparql ../../examples/Movies/table_1.csv
{% endhighlight %}

We do get the following RDF in N-Triples format.

{% highlight bash %}
<http://example.com/ns#1> <http://www.w3.org/1999/02/22-rdf-syntax-ns#type> <http://dbpedia.org/ontology/Film> .
<http://example.com/ns#1> <http://dbpedia.org/property/number> "1"^^<http://www.w3.org/2001/XMLSchema#integer> .
<http://example.com/ns#1> <http://dbpedia.org/property/released> "2009-12-18T00:00:00.000Z"^^<http://www.w3.org/2001/XMLSchema#dateTime> .
<http://example.com/ns#1> <http://www.w3.org/2000/01/rdf-schema#label> "Avatar" .
<http://example.com/ns#1> <http://dbpedia.org/property/budget> "425000000"^^<http://dbpedia.org/datatype/usDollar> .
<http://example.com/ns#1> <http://dbpedia.org/property/grossDomestic> "760507625"^^<http://dbpedia.org/datatype/usDollar> .
<http://example.com/ns#1> <http://dbpedia.org/property/grossWorldwide> "2783918982"^^<http://dbpedia.org/datatype/usDollar> .
<http://example.com/ns#2> <http://www.w3.org/1999/02/22-rdf-syntax-ns#type> <http://dbpedia.org/ontology/Film> .
<http://example.com/ns#2> <http://dbpedia.org/property/number> "2"^^<http://www.w3.org/2001/XMLSchema#integer> .
<http://example.com/ns#2> <http://dbpedia.org/property/released> "2007-05-24T00:00:00.000+01:00"^^<http://www.w3.org/2001/XMLSchema#dateTime> .
<http://example.com/ns#2> <http://www.w3.org/2000/01/rdf-schema#label> "Pirates of the Caribbean: At World's End" .
<http://example.com/ns#2> <http://dbpedia.org/property/budget> "300000000"^^<http://dbpedia.org/datatype/usDollar> .
<http://example.com/ns#2> <http://dbpedia.org/property/grossDomestic> "309420425"^^<http://dbpedia.org/datatype/usDollar> .
<http://example.com/ns#2> <http://dbpedia.org/property/grossWorldwide> "960996492"^^<http://dbpedia.org/datatype/usDollar> .
<http://example.com/ns#3> <http://www.w3.org/1999/02/22-rdf-syntax-ns#type> <http://dbpedia.org/ontology/Film> .
<http://example.com/ns#3> <http://dbpedia.org/property/number> "3"^^<http://www.w3.org/2001/XMLSchema#integer> .
<http://example.com/ns#3> <http://dbpedia.org/property/released> "2012-07-20T00:00:00.000+01:00"^^<http://www.w3.org/2001/XMLSchema#dateTime> .
<http://example.com/ns#3> <http://www.w3.org/2000/01/rdf-schema#label> "The Dark Knight Rises" .
<http://example.com/ns#3> <http://dbpedia.org/property/budget> "275000000"^^<http://dbpedia.org/datatype/usDollar> .
<http://example.com/ns#3> <http://dbpedia.org/property/grossDomestic> "448139099"^^<http://dbpedia.org/datatype/usDollar> .
<http://example.com/ns#3> <http://dbpedia.org/property/grossWorldwide> "1079343943"^^<http://dbpedia.org/datatype/usDollar> .
...
{% endhighlight %}

Last Update: {{ site.time | date: '%B %d, %Y' }}
