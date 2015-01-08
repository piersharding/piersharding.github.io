---
layout: post
title:  "Examples of tables and SAP::Rfc"
date:   2002-05-03 12:00:00
categories: general
---


I have had a number of emails lately about using SAP::Rfc, and the most common question I get asked is "How do I put entries in a table for an RFC call".  So here is a example ( that I should add into the documentation ):
<br/>
All complex parameters ( import parameters with structures ) and tables have
a helper class SAP::Struct.  The idea of this is to assist the user to create correctly structured rows that can then be passed in as values ( if you allready know the structure of your table or parameter, and want to create it using a pack() or some such then go ahead ).
<br/>
eg.
<pre>
# create an SAP connection
my $rfc = new SAP::Rfc( .... );

# discover the interface parameters etc.
my $i = $rfc->discover("BAPI_SALESORDER_CREATEFROMDAT1");

# grab the structure object for the table
my $str = $i->tab('ORDER_PARTNERS')->structure();

# set the values of the fields of the structure
$str->PARTN_VALUE("SP");
$str->PARTN_NUMB("44444444");

# add the correctly formated string representing the table row to an array
push(@rows, $str->value);
$str->PARTN_VALUE("AB");
$str->PARTN_NUMB("55555555");
push(@rows, $str->value);

# assign the array reference as the value of the table
$i->ORDER_PARTNERS( \@rows );

# do the call
$rfc->callrfc($i);
</pre>
So the SAP::Struct object $str helps you construct the full string value
for a row of the table ORDER_PARTNERS with all the values correctly
padded out etc.  This is especially useful for tricky SAP data types like 
 INT1, HEX etc. as the SAP::Struct class takes care of this for you.
then you accumulate these rows into a table and pass it into the
interface object as an array ref \@rows.

Once the call is completed, and you want to get hold of the contents of a
table you do something like:

<pre>
foreach my $row ( $i->tab('ORDER_PARTNERS')->hashRows() ){
  print $row->{'PARTN_VALUE'}."\n";
}
</pre>

<div id="a000022more"><div id="more">

</div></div>

<p class="posted">Posted by PiersHarding at May  3, 2002  2:26 PM</p>





