# Export-Obsidian.ps1 (v1.1.2)
- This is an old, unpublished PowerShell script I made for exporting specific Obsidian notes
and correlated attachments.
	- I'll likely make a new, refined one for the ``Obsidian-to-mdBook`` workflow, as well as automated ``SUMMARY.md`` file generation.
- This page is mostly to see how code block formatting looks on a test site.

```powershell
function Export-Obsidian {
#.SYNOPSIS
# Export Obsidian Note along with attachments to a specified directory.
# ARBITRARY VERSION NUMBER:  1.1.2
# AUTHOR:  Tyler McCann (@tylerdotrar)
#
#.DESCRIPTION
# Exports Obsidian markdown Notes and correlated attachments to a specified
# directory.  The exported Note is modified to point all attachments to the
# exported attachments directory, while creating a backup of the unedited
# original.
#
# Parameters:
#    -Target         -->   Obsidian Note to export
#    -AttachmentDir  -->   Directory containing Note's attachments
#    -OutputDir      -->   Export destination
#    -UrlEncoded     -->   Changes Obsidian embeds to URL encoded embeds
#    -Help           -->   Return Get-Help information
#
# Example Usage:
#    # Extract Note in a network share and export to current directory
#    Export-Obsidian -Target V:/Obsidian/CoolNote.md -AttachmentDir V:/Obsidian/attachments -OutputDir .
# 
#    # Output Directory
#     |
#     |__ attachments
#     |   |__ Pasted image 202307071.png
#     |   |__ Pasted image 202307072.png
#     |   |__ Pasted image 202307073.png
#     |
#     |__ CoolNote.md
#     |__ CoolNote.md.bak
#    
#.LINK
# https://github.com/tylerdotrar/<tbd>


    Param(
        [string]$Target,
        [string]$AttachmentDir, # = "<hardcoded_attachment_directory>",
        [string]$OutputDir, # = "<hardcoded_output_directory>",
        [switch]$UrlEncoded,
        [switch]$Help
    )
    

    # Return Get-Help Information
    if ($Help) { return (Get-Help Export-Obsidian) }


    # Error Correction
    $OutputDir = (Get-Item $OutputDir).FullName
    if (!$Target)        { return (Write-Host 'No target specified.' -ForegroundColor Red) }
    if (!$AttachmentDir) { return (Write-Host 'No attachment directory specified.' -ForegroundColor Red) }
    if (!$OutputDir)     { return (Write-Host 'No output directory specified.' -ForegroundColor Red) }

    $Target        = (Get-Item $Target).FullName 2>$NULL
    $AttachmentDir = (Get-Item $AttachmentDir).FullName 2>$NULL
    $OutputDir     = (Get-Item $OutputDir).FullName 2>$NULL

    if (!(Test-Path -LiteralPath $Target))        { return (Write-Host 'Target does not exist.' -ForegroundColor Red) }
    if (!(Test-Path -LiteralPath $AttachmentDir)) { return (Write-Host 'Attachment directory does not exist.' -ForegroundColor Red) }
    if (!(Test-Path -LiteralPath $OutputDir))     { return (Write-Host 'Output directory does not exist.' -ForegroundColor Red) }


    # Attempt to make folder to export attachements
    Try { mkdir "$OutputDir/attachments" -Force | Out-Null }
    Catch { return (Write-Host 'Error when creating exported attachments directory.' -ForegroundColor Red) }

    
    # Confirm Settings
    if ($UrlEncoded) { $FormatBool = 'TRUE'  }
    else             { $FormatBool = 'FALSE' }

    Write-Host "Exporting and Formatting Obsidian Note:" -ForegroundColor Green
    Write-Host "- Target Markdown File:        '$Target'"
    Write-Host "- Use URL Encoded Embed Paths: '$FormatBool'"
    Write-Host "- Export Directory:            '$OutputDir'"


    # Create a backup of the MD file without changes
    $Filename = (Get-Item -LiteralPath $Target).Name
    Copy-Item -LiteralPath $Target "$OutputDir/$Filename.bak" -Force
    Write-Host "- Target Backup:               '$Filename.bak'"


    # Modify embed paths and export
    $MdContents = Get-Content -LiteralPath $Target
    if ($UrlEncoded) { $URLpath = ($OutputDir.Replace(' ','%20')).Replace('\','/') }
    

    # Create Array of all Embedded Images to Export
    $EmbeddedImages = $MdContents | Select-String "Pasted image \d+.png" -AllMatches | % { ($_.Matches.Value -Split ' ')[-1] }
    

    # Export Embedded Images
    foreach ($Image in $EmbeddedImages) {
        
        Copy-Item -LiteralPath "$AttachmentDir/Pasted image $Image" "$OutputDir/attachments/Pasted image $Image" -Force
        Write-host "- Embedded Image Exported:     'Pasted image $Image'"
        

        # Change embed paths to URL encoded absolute path format
        if ($UrlEncoded) { $ExportedContents = $MdContents.Replace("![[Pasted image $Image]]","![](file://$URLpath/attachments/Pasted%20image%20$Image)") }


        # Change embed paths to point to new attachment folder
        else { $ExportedContents = $MdContents.Replace("![[Pasted image $Image","![[./attachments/Pasted image $Image") }


        Set-Content -Path "$OutputDir/$Filename" -Value $ExportedContents
        $MdContents = $ExportedContents
        
	    # Added wait to prevent hanging
		Start-Sleep -Milliseconds 200
    }

    return (Write-Host "Export complete." -ForegroundColor Green)
}
```