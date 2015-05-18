title: "Excel2JSON(php版)"
date: 2014-09-19 16:32:34
tags:
id: 731
comment: false
categories:
  - 编程学习
---

<pre class="brush:php">&lt;?php
    error_reporting(E_ALL);
    ini_set('display_errors', TRUE);
    ini_set('display_startup_errors', TRUE);
    define('EOL',(PHP_SAPI == 'cli') ? PHP_EOL : '&lt;br /&gt;');
    date_default_timezone_set('Europe/London');
    require 'PHPExcel/Classes/PHPExcel.php';
    require_once 'PHPExcel/Classes/PHPExcel/IOFactory.php';

    if(count($argv) &lt; 3){
        print("Usage: php xlsx2json.php xlsxFile v|h o|outfile\n");
        exit(1);
    }
    $path = $argv[1];

    if(!file_exists($path)){
        print("xlsx not find\n");
        exit(1);
    }

    function saveJsonFile($jsonData,$fileName)
    {
        $fp = fopen($fileName,"w");
        if($fp){
            $flag = fwrite($fp,$jsonData);
            if($flag)
            {
                fclose($fp);
                echo "s";
            }
        }
    }

    function xlsx2json($fileName,$dir){

        $objPHPExcel = PHPExcel_IOFactory::load($fileName);
        foreach ($objPHPExcel-&gt;getWorksheetIterator() as $worksheet) {
            $worksheetTitle     = $worksheet-&gt;getTitle();
            $highestRow         = $worksheet-&gt;getHighestRow(); // e.g. 10
            $highestColumn      = $worksheet-&gt;getHighestColumn(); // e.g 'F'
            $highestColumnIndex = PHPExcel_Cell::columnIndexFromString($highestColumn);
            $arr=array();
            $key = array();
            $valueArray = array();
            if($dir == "v"){
                for ($row = 1; $row &lt;= $highestRow; ++ $row) {
                    $ver=array();
                    for ($col = 0; $col &lt; $highestColumnIndex; ++ $col) {
                        $cell = $worksheet-&gt;getCellByColumnAndRow($col, $row);
                        $ver[] = $cell-&gt;getValue();
                    }
                    $key[] = $ver[0];
                    $valueArray = array_slice($ver,1);
                    $arr[] = array($ver[0] =&gt; $valueArray);
                }
            }else{
                for ($col = 0; $col &lt; $highestColumnIndex; ++ $col) {
                    $hor = array();
                    for ($row = 1; $row &lt;= $highestRow; ++ $row) {
                        $cell = $worksheet-&gt;getCellByColumnAndRow($col, $row);
                        $hor[] = $cell-&gt;getValue();
                    }
                    $key[] = $hor[0];
                    $valueArray = array_slice($hor,1);
                    $arr[] = array($hor[0] =&gt; $valueArray);
                }
            }
            //var_dump($arr);
            //echo json_encode($valueArray);

        }
        return $arr;
    }
    if($argv[3]){
        saveJsonFile(json_encode(xlsx2json($path,$argv[2])),str_replace('.xlsx','.txt',$path));
    }else{
        var_dump(xlsx2json($path,$argv[2]));
    }
?&gt;</pre>