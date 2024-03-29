Param(

[Parameter(Mandatory=$False)]
[string]$OutputPath, 

[Parameter(Mandatory=$False)]
[string]$AnswerFilePath,

[Parameter(Mandatory=$False)]
[string]$SelectSTIG,

[Parameter(Mandatory=$False)]
[string]$ExcludeSTIG,

[Parameter(Mandatory=$False)]
[string]$ScanType,

[Parameter(Mandatory=$False)]
[string]$OutputFileType,

[Parameter(Mandatory=$False)]
[string]$PreviousToKeep
)

<#
******************************************************
Version 1 

For this script to work, the EVAL stig path below has to be
present on the system the script executes against. EvalSTIG
is deployed as an application to the Eval STIG collection. 
Should maintain a persistenct copy of the folder on the boxes
in the collection


Version 2
This version accommodates changes introduced in Evaluate-STIG 1.2401.0
It added support for outputing checklists in the CKLB format 
used by STIG Viewer 3.
These newer parameters will crash previous versions of Evaluate-STIG.
So we test for the Evaluate-STIG Version, and run the appropriate
script version.

Parameters Added: 
-Output (Specify outputs to generate. Multiple outputs may be 
specified though comma separation. By default, results are 
output to a PowerShell object within the console. If outputting 
to .CKL, .CKLB, or STIGManager, no results are returned to the 
console. Required for -PreviousToKeep, and -OutputPath. Default Used: "CKL")

-PreviousToKeep (Number of previous scan session outputs to retain in -OutputPath. Scan outputs are
the items requested with -Output. Using -PrevousToKeep 0 will remove all
previous results and only retain current results. Retained results will be moved to a
[OutputPath]\Previous\[Results Date-Time] folder. Default Used: "1")

******************************************************
#>
start-transcript -Path "C:\SCAP\transcript.txt"

#     Include the path where EVAL-STIG should be located for convenience
#     you better not have spaces in this path
$EvalSTIGPath = ((Get-ChildItem C:\SCAP\ -Filter *Evalu*)[0]).FullName
$EvalSTIGPathPS1 = "$EvalSTIGPath\Evaluate-STIG.ps1"

#     Test for empty parameters that have default value overrides.
if(!$ScanType) {$ScanType = 'Unclassified'};
if(!$OutputFileType) {$OutputFileType = 'CKL,CKLB'};
if(!$PreviousToKeep) {$PreviousToKeep = '2'};

#     Build the Arguments to be passed to the EVAL-STIG script
$OutputPath_ARG = "-OutputPath" + ' ' + '"' + $OutputPath + '"'
$AFPath_ARG = "-AFPath" + ' ' + '"' + $AnswerFilePath + '"'
$SelectSTIG_ARG = "-SelectSTIG" + ' ' + '"' + $SelectSTIG + '"'
$ExcludeSTIG_ARG = "-ExcludeSTIG" + ' ' + '"' + $ExcludeSTIG + '"'
$ScanType_ARG = "-ScanType" + ' ' + $ScanType
$Output_ARG = "-Output" + ' ' + '"' + $OutputFileType + '"'
$PreviousToKeep_ARG = "-PreviousToKeep" + ' ' + $PreviousToKeep

#     Test if the parameters are $null; if null, delete the variable so we don't pass null variables to the Script and botch everything up
if(!$OutputPath) {Remove-Variable OutputPath_ARG};
if(!$AnswerFilePath) {Remove-Variable AFPath_ARG};
if(!$SelectSTIG) {Remove-Variable SelectSTIG_ARG};
if(!$ExcludeSTIG) {Remove-Variable ExcludeSTIG_ARG};


#     test the path to the EVALStig folder
if (Test-Path -Path $EvalSTIGPath) {

    #     Strip the version from $EvalSTIGPath
    $EvalSTIGVersion = $EvalSTIGPath -replace "[^0-9]"

    #     Test for older versions of Eval stig
    if ($EvalSTIGVersion -lt "124010") {

        #     Compile the argument list into one string to pass to the script
        $argus = $ScanType_ARG + " " + $OutputPath_ARG + " " + $AFPath_ARG + " " + $SelectSTIG_ARG + " " + $ExcludeSTIG_ARG 
        write-host "argument list built, passing $args"

        cd $EvalSTIGPath
        write-host  "starting evaluate STIG - NOW"
        invoke-expression "$EvalSTIGPathPS1 $argus"
        write-host "theoretically, the process should be completed"
    } 

    else {

        #     Compile the argument list into one string to pass to the script
        $argus2 = $ScanType_ARG + " " + $Output_ARG + " " + $PreviousToKeep_ARG + " " + $OutputPath_ARG + " " + $AFPath_ARG + " " + $SelectSTIG_ARG + " " + $ExcludeSTIG_ARG 
        write-host "argument list built, passing $args"
    
        cd $EvalSTIGPath
        write-host  "starting evaluate STIG - NOW"
        invoke-expression "$EvalSTIGPathPS1 $argus2"
        write-host "theoretically, the process should be completed"
    }
} 

else {
write-host "cannot find $EvalSTIGPath, Exiting"
}

-OutputPath ‘"\\ServerName \ShareName"' -AnswerFilePath  ‘“\\ServerName\ShareName\AnswerFiles"' -ScanType "Unclassified"

NOTE: BE SURE TO WRAP FILE PATHS IN DOUBLE QUOTES, AND AGAIN IN SINGLE QUOTES. IF SINGLE QUOTES ARE NOT ENCASING THE DOUBLE QUOTED PATH, THEN FILE PATHS CONTAINING SPACES WILL NOT PASS CORRECTLY.

