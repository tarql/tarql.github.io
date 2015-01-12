---
layout: page
title: Examples
permalink: /examples/
---

### Example \#1

#### CSV data

The following lines are part of a CSV file ([TechCrunchcontinentalUSA.csv](https://github.com/emir-munoz/tarql/blob/master/examples/TechCrunchcontinentalUSA.csv)) 
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
~$ sh bin/tarql --ntriples ../../examples/sample-2.sparql ../../examples/TechCrunchcontinentalUSA.csv
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

Last Update: {{ site.time | date: '%B %d, %Y' }}
