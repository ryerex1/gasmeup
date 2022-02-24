# gasmeup

	
	****************************
		GASMEUP GUIDE:
	****************************
	
	Overview:
	=========
	
	Hey there! Ethereum (ETH) can be wild and intimidating, so let's break it down to its lifeblood: gas.
	This project allows the user to obtain the gas prices (in 0.1 Gwei) required to complete a transaction on the ETH blockchain, 
	as well as the average gas price over a specified period of time (with Unix timestamps).
	
	Why does this matter?
	=====================
	
	If you have ever performed a transaction on the ethereum blockchain, you paid for that transaction with gas. 
	It's usually a small fee. But if you need to conduct transactions on the blockchain numerous times, the small gas fees can add up quickly. 
	This project can help you obtain insights on which moment is more cost-effective to submit your transaction to the blockchain. 
	Ethereum has also recently been criticized for soaring gas prices, which means that obtaining insights on gas prices can also give you an eye 
	into global ethereum adoption and acceptability. Whether you're an ETH miner, someone who needs to send a transaction, 
	or a financial analyst curious about blockchain, knowing gas prices at given points and over specified time intervals 
	can help you perform your task more efficiently.
	
	Some notes before beginning:
	===========================
	
	* Units on the /gas and /average endpoints are measured in 0.1 Gwei. For Gwei amounts, divide the number by 10. (1 Gwei is 10e-9 of 1 Ether)
	* I wrote the GasMeUp app in Node.js, as I was slightly familiar with Javascript and less so with Typescript and C#. 
	I chose Express as this is the leading server framework today, especially good for JSON APIs. 
	In addition, I chose MySQL as the receiving database also because of its familiarity.
	
	Without further ado... drumroll please.........
	
	How to Gas Yourself Up (aka run the app):
	=========================================
	
	The app, GasMeUp, is containerized on Docker. To run it, download the zipped file in my email and extract it into any directory. 
	Then within this directory, run the command �docker-compose -f "docker-compose.yml" up -d --build� from a command terminal. 
	This will build the application and database (I chose MySQL) necessary to retrieve gas price information. 
	The app will now be running on http://localhost:3000
	
	Once the app is running, you have three options.
	
	* Entering "localhost:3000" into your browser will retrieve this README file (hi!).
	* Adding on the "/gas" endpoint (i.e. "localhost:3000/gas") will give you the gas price information at that moment.
	* Adding the formatted "/average" endpoint will give you the average gas price over the timepoints you specify. 
		(The format should be "/average?fromTime=xxxxxxxxxx&toTime=xxxxxxxxxx" where xxxxxxxxxx is the Unix timestamp in 
		seconds for the fromTime and toTime, respectively.)
	* For the running average gas price from the beginning, you can simply make fromTime the timestamp at which you 
		deployed the code and toTime the timestamp 10 minutes into the future. This allows you to simply continue refreshing the page.
	
	Interpreting the data:
	======================
	
	* For the /gas endpoint, it first displays whether an error is present. If you see "error": false, that means it's working properly. (Congratulations!) 
		Then, it will display the three gas prices in 0.1 Gwei for the fastest, average, and slowest rate of transaction approval respectively, 
		followed by the number of the latest block added to the chain ("blockNum")
	* "fast" is actually an average of the lowest gas price accepted by top miners in the pool; anything higher will not speed up transactions much more. 
		If you have a high-priority transaction, this is near the best you're going to get.
	* "average" is the price accepted by miners who have contributed to mining at least 50% of the most recent 100 blocks
	* "low" is the lowest price where at least 5% of the network hash power will accept it. 
		This gas price should be paid for very low priority transactions.
	
	Some notes and clarifications:
	==============================
	
	* The second endpoint parameter "toTime" is allowed to be up to 10 minutes into the future, 
		but the "average" displayed is always only up to the current time (because that's the only information in the database so far). 
		If I had more time, I would return another error message below this, explaining how the second timepoint has not been reached yet. 
		I chose to do this so that the user can simply set the second timepoint to the end time and, conveniently keeping the query the same, 
		the user can obtain results that update every time they refresh the page.
	* When probing and inserting the gas price data into mySQL database, I manually set the refresh rate (sleep) to 5 seconds. 
		I was aware that there was an API rate limit on EthGasStation, and after manually refreshing the page, 
		I figured 5 seconds was approximately EthGasStation�s refresh rate as well. 
		Also, for this reason, I didn�t want to use a Websocket and chose an API instead.
	* I chose EthGasStation over EtherScan because it seemed like EthGasStation was primarily focused on gas prices, 
		where Etherscan was more of an entire blockchain explorer. I preferred the specialized one. However, with more time, 
		I might look into the differences between the two or find if there's a more accurate third one to use. 
		I already saw some subreddits saying Etherscan was better...
	* The two node packages "request" and "request-promise" are both deprecated. With more time, I would seek alternative modules.
	* For other notes, see the comments within the code itself!
	
	
	* I tried to take into account any errors that might occur while deploying the code, but there is always room for improvement. 
		For any errors or suggestions, please email rx2190@columbia.edu. I appreciate any feedback; after all, criticism makes the app more robust!
	
	Conclusion:
	===========
	
	Thanks for using my app! Hope you were able to obtain the insights you were looking for. 
		This was very fun for me too, learning how to fully make an app, from having it communicate with web servers to containerizing the whole project. 
		Shoutout to AlphaPoint for pitching me this challenge!
	
	
