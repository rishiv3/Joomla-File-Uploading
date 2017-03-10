# Joomla-File-Uploading
File Uploading in the Joomla Custom Component with php script 

For uploading the file you need to write your code in the controller file and you can extend the save() method. check the code given below -

````php
public function save($data = array(), $key = 'id')
{
    // Neccesary libraries and variables
    jimport('joomla.filesystem.file');

    //Debugging 
    ini_set("display_error" , 1);
    error_reporting(E_ALL);

    // Get input object
    $jinput = JFactory::getApplication()->input;

    // Get posted data
    $data  = $jinput->get('jform', null, 'raw');
    $file  = $jinput->files->get('jform');

    // renaming the file 
    $file_ext=strtolower(end(explode('.',JFile::makeSafe($file['pdf_file_path']['name']))));
    $filename = round(microtime(true)) . '.' . $file_ext;
    // Move the uploaded file into a permanent location.
    if ( $filename != '' ) {

        // Make sure that the full file path is safe.
        $filepath = JPath::clean( JPATH_ROOT."/media/your_component_name/files/". $filename );

        // Move the uploaded file.
        if (JFile::upload( $file['pdf_file_path']['tmp_name'], $filepath )) {
              echo "success :)";
        } else {              
              echo "failed :(";
        }        
    }

    JRequest::setVar('jform', $data, 'post');
    $return = parent::save($data);
    return $return;
}
```
