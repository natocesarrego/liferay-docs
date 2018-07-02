# Data Providers

Select from List fields can hold a lot of options. There are around 200
countries on earth, for example. If you have unoccupied unpaid interns you could
ask them to type each country into the Select from List field manually, or you
could auto-populate your select fields using a REST web service. This saves you
(or your interns) the trouble of typing all those options, and you can rely on
someone else (hopefully a trustworthy expert) to keep the data updated.

When setting up a data provider, you're accessing a 
[REST web service](https://en.wikipedia.org/wiki/Representational_state_transfer). 
Use the 
[JSON web services registered in Liferay](/develop/tutorials/-/knowledge_base/7-0/registering-json-web-services),
or any other REST web service you have access to. To find a list of the
ready-to-use registered JSON web services in @product@, navigate to
[http://localhost:8080/api/jsonws](http://localhost:8080/api/jsonws) 
(assuming you're running a local server). Browse the available Liferay services.
Many times, the services useful to you in the Forms application will get a list
of something. Find the `get-countries` JSON web service and click on it, then
click *Invoke*. The *Result* tab shows a list of countries using JSON syntax,
like this:

    [
      {
        "a2": "AF",
        "a3": "AFG",
        "countryId": "20",
        "idd": "093",
        "mvccVersion": "0",
        "name": "afghanistan",
        "nameCurrentValue": "Afghanistan",
        "number": "4"
      },
        ...

That's the record for the country Afghanistan. As you can see in the *URL* tab,
the URL you entered into the data provider form is the same as the one generated
for accessing the `get-countries` JSON web service. Find the URL for any
registered JSON web service using this same procedure. 

## Adding a Basic Data Provider

To add a *Countries of the World* Data Provider for use in your Forms:

1.  Go to the Forms application.

2.  Click the Options button (![Options](../../../images/icon-option.png)) and
    click *Data Providers*.

3.  Click the Add button (![Add](../../../images/icon-add.png)).

    The REST Data Provider form loads.

4.  Fill out the Name and Description fields.

    Name: `Countries of the World`

5.  Enter the URL and authentication tokens for the REST service. For the
    `get-countries` service:

    URL: `http://localhost:8080/api/jsonws/country/get-countries/`

    User Name: `test@liferay.com`

    Password:`test`

6.  In the Outputs fields, specify which field from the REST service is
    populating your select list. 

    Label: `Country Name`

    Path: `nameCurrentValue` 

    Type: `List`

7.  Save the Data Provider.

SCREENSHOT

<!--    provides the full, properly capitalized country name, and that’s what you want users of the form to see. We are using JsonPath to navigate the returning xml structure. JsonPath uses special notation to represent nodes and their connections to adjacent nodes in a path. There are two styles of notation, namely dot and bracket.
You can check more detail on this topic on the links showing in your screen:
http://jsonpath.com/

http://goessner.net/articles/JsonPath/

https://www.pluralsight.com/blog/tutorials/introduction-to-jsonpath

I'm using the same service but for outputing the EU countries only. The Path in the outputs needs to be Json Path. Use this website http://jsonpath.com/? to test the output and understand the expression you need to have.
I'm currently recording the videos for our training and the chapter on data providers looks like the text below.

Hope this helps!
RL
-->

## Using a Data Provider in a Select Field [](id=using-a-data-provider-in-a-select-field)

Once the Data Provider is configured, use it to populate a Select from List field:

1.  Go to the Form Builder (add a new form or edit an existing one)

2.  Drag a Select from List field onto the form.

3.  In the Create List section, choose *From Data Provider*.

4.  Choose the Data Provider and its Output Parameter:

    Choose a Data Provider: `Countries of the World`

    Choose an Output Parameter: `Country Name`

5.  Publish the form and test it. 

SCREENSHOT

Your Data Provider is now being used to populate a select field. However,
this form is going to be submitted by Guest users, who don't currently have
permission to see the list of results from the data provider.

## Granting Data Provider Permissions [](id=granting-data-provider-permissions)

To configure the data provider's permissions, go to the Forms application (*Site
Administration* &rarr; *Content* &rarr; *Forms*). Open the Options menu
(![Options](../../../images/icon-options.png)) and select *Data Providers*. For
the data provider you want to configure, click the Actions button
(![Actions](../../../images/icon-actions.png)), then *Permissions*. 

Configure the permissions you need. If Guests are to fill out the form, they
need the *View* permission, or else they won't be able to see the options
provided by the data provider. Once you grant permissions, click *Save*.

## Data Provider Configuration

The above instructions for adding a basic Data Provider are a good start, but
there are more options.

*URL* : The URL of a REST Web Service, which can be an internal or an external REST web service. In both cases, the URL can receive two types of parameters, the path parameters and the query parameters. The path parameters make part of the URL and are added following the pattern `{path_parameter_name}`, like in `https://restcountries.eu/rest/v2/name/{name}` where the `{name}` is the path parameter. On the other hand, the query parameters are used as a complementary part of the URL and follows the pattern `?query_parameter=query_parameter_value` like in `https://restcountries.eu/rest/v2/name/{name}?query_parameter=query_parameter_value`.

*User Name* and *Password* : Credentials used to autheticate in the REST Web Service. These credentials are valid for internal and external REST Web Services.

*Support filtering by keyword.*
: If activated<!-- what does this do?--> Enter a valid parameter from the REST service into the
Filter Parameter Name. This parameter is used to filter the REST service's
results.

When this parameter is enabled, a new field named `Filter Parameter Name` is displayed in the Data Provider configuration. This field receives the name of a query parameter, which will be used to filter the results of the Data Provider. It's important to emphasize that this field only supports a single query parameter.

*Cache data on the first request.*
: If the data is cached, a second load of the select list field is much faster,
since a second call to the REST service provider is unnecessary.

The cache activated by this field is the cache provided by Liferay Portal, there is no special cache implemented by Forms.

*Timeout*
: The time (in ms) to allow the REST service call to process before aborting the
request, if a response is not returned.

If the REST service call timeouts, then a thirty party tool ([Hystrix](https://github.com/Netflix/Hystrix)) is used by Forms to disable the communication with the REST service temporarily unavailable.

*Inputs*
: Specify <!--what does this do? --> Enter a Label, the Parameter that ............, and the
Type (Text or Number). Choose whether the input is required. You can add
multiple Inputs.

*Outputs*
: You can add multiple Outputs.


