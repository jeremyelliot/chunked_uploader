Chunked_uploader
===============
CodeIgniter Spark library for handling chunked uploads from Plupload
------------------------------------------
An object-oriented adaptation of the PHP demo server for Plupload 
by Moxiecode Systems AB

http://www.plupload.com/

Released under GPLv2 License (http://www.gnu.org/licenses/gpl-2.0.html)

Usage example
-------------
This shows a very basic controller method to receive files
uploaded from a browser using Plupload.

I'm using a `Json_rpc_response` object for the response, you might prefer
to do it a different way.

	public function plupload_server()
	{
		error_reporting(0);
		$config = array(
				'max_tmp_file_age' => 900,
				'max_execution_time' => 180,
				'target_dir' => 'uploads');
		$this->load->spark('chunked_uploader/0.0.1');
		$this->load->library('Chunked_uploader', $config, 'uploader');
		$this->load->helper('json_rpc');

		try
		{
			$this->uploader->upload();
			if ($this->uploader->is_completed())
			{
				do_something_with_uploaded_file($this->uploader->get_file_path());
				$response = new Json_rpc_response('success');
			}
			else
			{
				$response = new Json_rpc_response('continue');
			}
		}
		catch (RuntimeException $ex)
		{
			$response = new Json_rpc_error_response($ex->getMessage());
		}
		$response->send();
	}
