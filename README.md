# Improved-NetSuite-Search

NetSuite provides a basic search function (nlapiSearchRecord), which by default returns a maximum of 1000 results.

Searching more than 1000 records is possible by utilizing while loops and a new filter consisting of the internal ID of the last search result.

This commonly used method has the possibliity of data loss, if the 1000th result is a multi line record.  

Example: Result 1000 == transaction 100, line 4 (out of 10).  The next search will pull transaction 101, line 1.  While skipping transaction 100 line 5 - 10.

Example Usage:

		var Filter	= new Array();
		Filter[0] 	= new nlobjSearchFilter('trandate', null, 'within', firstDate, lastDate);
		Filter[1]	  = new nlobjSearchFilter('posting', null, 'is', 'T');
		
		var Column 	= new Array();
		Column[0] 	= new nlobjSearchColumn('internalid').setSort();
		Column[1]   = new nlobjSearchColumn('line').setSort();
		Column[2] 	= new nlobjSearchColumn('trandate');
		Column[3] 	= new nlobjSearchColumn('account');
		Column[4] 	= new nlobjSearchColumn('amount');
		Column[5] 	= new nlobjSearchColumn('entity');
		Column[6] 	= new nlobjSearchColumn('type');
		
		var results = search(Filter, Column, 'transaction');

The above will return up to 100,000 results in Suitelets, or 1,000,000 in Scheudled scripts.
