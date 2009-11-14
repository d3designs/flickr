# Flickr API

This API will construct the appropriate Flickr REST API URL to query, and use RequestCore and SimpleXML to retrieve and parse the data (by default).

## Requirements

* PHP 5.2
* cURL
* SimpleXML
* [RequestCore](http://github.com/skyzyx/requestcore)

## Download

	git clone git@github.com:skyzyx/flickr.git
	cd flickr
	git submodule init
	git submodule update

## Setup

You can rename `config-sample.inc.php` to `config.inc.php` and add your key/secret there, or you can pass your key/secret key to the constructor.

I would recommend the former over the latter if you generally only use one key/secret set.

## Example usage

If you want to make a request to Flickr's `flickr.people.findByUsername` method, you'd do the following. This makes a request using [RequestCore](http://github.com/skyzyx/requestcore), defaults to an XML response from Flickr, and parses it with SimpleXML.

	$flickr = new Flickr();
	$response = $flickr->people->find_by_username(array(
		'username' => 'skyzyx'
	));
	print_r($response);

You can look through the response to see how to traverse through the data.

If you want to use a different response format, you'll also want to override the `parse_response()` method.

	class FlickrPHP extends Flickr
	{
		public function parse_response($data)
		{
			return unserialize($data);
		}
	}

	$flickr = new FlickrPHP();
	$response = $flickr->people->find_by_username(array(
		'username' => 'skyzyx',
		'format' => 'php_serial'
	));
	print_r($response);

This will give you working PHP data to iterate over. You can also use this override technique to switch from <code>SimpleXML</code> to <code>DOMDocument</code> or anything else you may want to use to parse the XML.

To use a different HTTP request/response class, you would override the <code>request()</code> method.

## License & Copyright

This code is Copyright (c) 2009, Ryan Parman. However, I'm licensing this code for others to use under the [MIT license](http://www.opensource.org/licenses/mit-license.php).
