## Web scrapping: olx.uz
# Working out errors related to the start of the code
- If an error occurs when running the code after installing the necessary libraries, then most likely the Chrome web driver for your OS is not installed. How to install the web driver can be found at this link. [selenium-python.com](https://selenium-python.com/install-chromedriver-chrome?ysclid=ly2ufnhjip111135754)
- If the code does not work after installing the web driver or an error occurs, then you need to check whether the Chrome browser is open in memory. If so, you need to unload it (close it). This is due to the fact that the Chrome browser launched by the code runs in the background in the PC memory and may conflict with the Chrome browser running at the same time.
- If the error still occurs, then you need to check or reinstall the libraries you are using.
# Working with the parser
- To start parsing the site [olx.uz](https://www.olx.uz/), ранее Torg: сайт объявлений в Узбекистане - купля/продажа бу товаров на OLX.uz  you need to make a request with a call to the parser Parser_olx().parser(url_request) in this case, the parser will collect site data from the page to the results of the url_request ad search query. In this case, the parser will start collecting links to ads from the address page that you entered and will automatically start collecting data from ads that links to which were collected. The result of the work will be an array of data from ad links.
- If the url_request was passed to the first page of the search result, then the parser itself will start an iterator through the remaining pages and reach the end of the search result that the site returned. If the url_request does not contain the first page, then data will be collected only from this search page without running the iterator. 
- For convenience, the parser can be run separately collecting links from the query result and separately collecting data from the ad links themselves. This approach helps to optimize data collection, for example, first collect only links, compare them with those results that were collected earlier and exclude them from data collection, leaving only unique new links for which data was not collected earlier and start data collection only from these links.
- To start collecting only ad links, you need to use the Parser_olx().search_parser(url_request) call where url_request is a link to the first page of the search result, the iterator will collect data on the rest of the pages itself. If you do not enter the first page of the search, the results will be collected only from this page. The result of the work will be an array of ad links.
- To start collecting data from ads, use the Parser_olx().parser(url_array) call, where url_array is an array of collected ad links. The result of the work will be an array of data from ad links.
- When initializing the parser class Parser_olx(), hyperparameters can be set that affect the speed of operation and set time delays: upload_wait_time - the waiting time for page loading after transmitting the URL and the maximum waiting time for loading elements on the page after interacting with the page to load data, sleep_time_min and sleep_time_max - set the time interval between which a random waiting interval is generated for simulating user actions of scrolling and the equivalent of waiting for information to be downloaded and read. parsing_stopper_min and parsing_stopper_max - set the time interval between which a random waiting interval is generated to simulate pauses between collecting a certain block of data or if the result of the previous data collection caught a captcha, parsing_stopper_count - determines the amount of data collected between pauses. This helps to be more similar to human actions when collecting information. If you initialize the class without hyperparameters, it will be automatically called with default parameters: sleep_time_min = 30, sleep_time_max = 120, upload_wait_time = 5, parsing_stopper_min = 300, parsing_stopper_max = 600, parsing_stopper_count = 10.
- If it is the user's phone number that is of interest when collecting information, then it is not recommended to make too small time intervals, since automatic protection will work on the site and the site will issue a captcha to the place where the number is displayed after clicking on the "Show number" button. The captcha will start to be displayed approximately after the 14th request, sometimes earlier, sometimes later. The format of the displayed captcha is reCAPTCHA. The best way to get around it is to avoid calling it. If the system notices parsing, the lock will usually turn on for about a day. In this case, it is recommended to reconnect the Internet and try again or run the parser later. If the captcha is called too often, it is better to increase the time intervals through the hyperparameters sleep_time_in, sleep_time_max, upload_wait_time, parsing_stopper_min, parsing_stopper_max, parsing_stopper_count.
- If a captcha is called when collecting information, the phone_not_parsed entry will be entered in the data_parsed column of the resulting frame. If necessary, it will be possible to filter out such values and run parsing on them again.
- If the user's phone number is not very interesting when collecting information, for example, information is interesting for automatic filtering and processing, and only then the phone number and the link to the ad are interesting, then the time delays sleep_time_min, sleep_time_max, upload_wait_time, parsing_stopper_min, parsing_stopper_max, parsing_stopper_count should be made minimal to speed up the parser. In this mode, we will catch a captcha, but this will not be a problem since the information on the phone number of the ad will not be interesting at the information collection stage.
- To save the resulting frame to a CSV file, you need to decompress the lines of code #df_pd = pd.DataFrame(df) which converts the array to Pandas Data Frame and #df_pd.to_csv(f"Selenium_olx_{datetime.now().strftime('%d%m%Y')}.csv") which saves the data to a file named "Selenium_olx_31072024.csv" in the folder with the code being run. The date will be inserted automatically - the current date of saving the parsing result.
# Important
- Before using parsing, think over the logic of what data and for what you plan to collect and build on this by setting time intervals and setting relevant amounts of information collection. It is probably wiser to collect information with other pauses. Remember, time affects the result! 
- The site has a limitation in the links provided. When issuing a search result, the site does not give out all the results, but a little more than 1000 most relevant to the query. It is extremely important for the best result to make the most correct search query with the necessary site settings and filters. These settings are automatically adjusted and added to the request link. Copy the link only after forming the correct request, only this will provide the result you need.
# Additionally
- This script is provided for educational and informational purposes only.
