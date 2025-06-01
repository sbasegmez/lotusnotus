---
date: 2025-05-27
categories:
    - welcome
---

# Hello World

How are you today?
<!-- more -->
## What is Lorem Ipsum?
Lorem Ipsum is simply dummy text of the printing and typesetting industry. Lorem Ipsum has been the industry's standard dummy text ever since the 1500s, when an unknown printer took a galley of type and scrambled it to make a type specimen book. It has survived not only five centuries, but also the leap into electronic typesetting, remaining essentially unchanged. It was popularised in the 1960s with the release of Letraset sheets containing Lorem Ipsum passages, and more recently with desktop publishing software like Aldus PageMaker including versions of Lorem Ipsum.

```java
    public static MetadataDefinition DEFAULT = new Builder()
                    .addString("form")
                    .addString("noteid", "@RightBack(@NoteID;\"NT\")")
                    .addString("unid", "@Text(@DocumentUniqueID)")
                    .addTemporal("created", "@Created")
                    .addTemporal("addedtofile", "@AddedToThisFile")
                    .addTemporal("lastmodified", "@Modified")
                    .addTemporal("lastmodifiedinfile", "@ModifiedInThisFile")
                    .addTemporal("lastaccessed", "@Accessed")
                    .addInteger("size", "@DocLength")
                    .build();
```

```formula
targetField := @Prompt([OkCancelEditCombo]; "Select Field"; "Select a field to override:"; ""; @DocFields );
updateTypes := "Text":"Number":"Time":"Delete Field";
updateType := @Prompt([OkCancelList]; "Select Type"; "Choose a type or action:"; "Text"; updateTypes );
@If(updateType = "Delete Field"; @Return(@SetField(targetField;@DeleteField)); "" );
newValue := @Prompt([OkCancelEdit];"New Value";"Enter the new value:"; @Text(@GetField(targetField )));
newTypeValue := @Select(@TextToNumber(@Replace(updateType; @Subset(updateTypes;3);"1":"2":"3" )); newValue; @TextToNumber(newValue); @TextToTime(newValue));
@SetField(targetField; newTypeValue)
```

```vbscript
Const APP_FORM = "Appointment"
Const DD_SERVER = "Serv01"
Const SEPERATOR = "!!"
 
Class CEntry
   
  Public Sub new()
  End Sub
   
  Private startdttm As NotesDateTime
  Private enddttm As NotesDateTime  
  Private moddttm As NotesDateTime
  Public Property Set StartDT As NotesdateTime
    Set startdttm = StartDT
  End Property
  Public Property Set EndDT As NotesdateTime
    Set enddttm = EndDT
  End Property
   
  Public Property Get AppType As String
    Select Case Ucase (Me.StrType)
    Case "APPOINTMENT", "TERMIN"
      AppType = "0"
    Case "ANNIVERSARY", "JAHRESTAG"
      AppType = "1"
    Case "EVENT", "GANZTAEGIGE VERANSTALTUNG"
      AppType = "2"
    Case "MEETING", "BESPRECHUNG"
      AppType = "3"
    Case "REMINDER", "ERINNERUNG"
      AppType = "4"
    Case Else
      AppType = "0"
    End Select
  End Property

  Public Property Get MailFile As String
    Dim s As New NotesSession
     
    If Me.struser = "" Then
      MailFIle = ""
    Else ' Notes Version is < 8.x
       
      If s.NotesBuildVersion < 307 Then
        Dim db As New NotesDatabase ( DD_SERVER, "names.nsf" )
        Dim v As NotesView
        Dim doc As NotesDocument
        If db.IsOpen() Then
          Set v = db.GetView("($Users)")
          If Not ( v Is Nothing ) Then
            Set doc = v.GetDocumentByKey (Me.user)
            If Not ( doc Is Nothing ) Then
              MailFIle = doc.MailServer(0) & SEPERATOR & doc.MailFile(0)
            Else
              Goto ERR_USER_NOT_FOUND
            End If
          Else
            Goto ERR_USER_NOT_FOUND
          End If
        Else
          Goto ERR_USER_NOT_FOUND
        End If
         
      Else ' we are running at least Notes Version 8
        On Error 4731 Goto ERR_USER_NOT_FOUND
        Dim notesdir As NotesDirectory
        Set notesdir  = s.getDirectory(DD_SERVER)
        Dim homeserver As Variant
        homeserver =  notesdir.GetMailInfo (Me.struser, False, False)
        mailfile = Cstr(homeserver(0)) & SEPERATOR & Cstr(homeserver(3))  
      End If     
EXIT_PROPERTY:
      Exit Property
ERR_USER_NOT_FOUND:
      mailfile = ""
      Resume EXIT_PROPERTY
    End If
     
  End Property
   
  Public Function CreateSingleEntry As Integer
    CreateSingleEntry = 0 ' no error
     
    If Trim(Me.strSubject) = "" Then
      CreateSingleEntry = 3 ' Subject missing
      Exit Function
    End If
     
    If Me.startdttm.TimeDifference (Me.enddttm) > 0 Then
      CreateSingleEntry = 4 ' EndDT before StartDT
      Exit Function
    End If
     
    If Me.MailFile = "" Then
      CreateSingleEntry = 1 'No MailFile or User not found
      Exit Function
    End If
     
    Dim db As New NotesDatabase ( _
    Strtoken (Me.Mailfile,SEPERATOR,1), Strtoken (Me.Mailfile,SEPERATOR,2)) 
     
    If db.IsOpen () Then
      Dim session As New NotesSession
      Dim nam As NotesName
      Dim cEntry As New NotesDocument (db)
      Dim rtitem As Variant
      Dim itemIcon As NotesItem
      Dim item As NotesItem
      Dim ret As Variant
       
      Set nam = session.CreateName(Me.struser)    
       
      '----------- Set User and Description ----------------
      cEntry.Form = APP_FORM
      cEntry.~$Programmatically = "1"
      'cEntry.tmpOwnerHW = "0"
      Set item = New NotesItem(cEntry, "From", nam.canonical)
      item.IsAuthors = True
      Set item = New NotesItem(cEntry, "Principal", nam.canonical)
      'Set item = New NotesItem(cEntry, "Chair", nam.canonical)
      Set item = New NotesItem(cEntry, "$BusyName", nam.canonical)
      item.IsNames = True
       
      cEntry.AppointmentType = Me.AppType
      Select Case Me.AppType
      Case 0
        Set itemIcon = New NotesItem(cEntry, "_ViewIcon", 160)
      Case 1
        Set itemIcon = New NotesItem(cEntry, "_ViewIcon", 63) 
      Case 2
        Set itemIcon = New NotesItem(cEntry, "_ViewIcon", 168)
      Case 3
        Set itemIcon = New NotesItem(cEntry, "_ViewIcon", 9)
      Case 4
        Set itemIcon = New NotesItem(cEntry, "_ViewIcon", 158)
      End Select
      itemIcon.IsSummary = True
       
      cEntry.~$BusyPriority = "1"
      cEntry.Subject = Me.subject
       
      '----------- Set Date and Times ----------------
      cEntry.StartDateTime = Me.startdttm.LSLocalTime
      cEntry.StartDate = Me.startdttm.LSLocalTime
      cEntry.StartTime = Me.startdttm.LSLocalTime
      cEntry.EndDateTime = Me.enddttm.LSLocalTime
      cEntry.EndDate = Me.enddttm.LSLocalTime
      cEntry.EndTime = Me.enddttm.LSLocalTime
      cEntry.calendarDateTime = Me.startdttm.LSLocalTime
       
      '----------- Set Other Fields ----------------
      cEntry.~$NoPurge = Me.enddttm.LSLocalTime
      cEntry.~$PublicAccess = "1"
      cEntry.MailOptions=""
      cEntry.tmpWhichList = ""
       
      Set item = New NotesItem(cEntry, "ExcludeFromView", "D")
      Call item.AppendToTextList ("S")
      cEntry.OrgTable = "C0"
      cEntry.Location = Me.strLocation
      Set item = New NotesItem(cEntry, "Categories", varCategories)     
      'cEntry.Categories = "Test"
      cEntry.Logo = "stdNotesLtr0"
      cEntry.OrgState = "x"
      cEntry.Repeats = ""
      cEntry.Resources = ""
      cEntry.SaveOptions = ""
      cEntry.SequenceNum = "1"
      cEntry.APPTUNID = cEntry.UniversalID
      '//Call cEntry.ComputeWithForm (False,True)
      Call cEntry.save(False, True)
       
    Else
      CreateSingleEntry = 2 'MalFile cannot be opened
    End If
  End Function
   
End Class
 
Sample Usage
 
Sub Click(Source As Button)
  Dim CEntry As New CEntry()
  CEntry.AppType = "Appointment"
  CEntry.User = "Beta User/Singultus"
  CEntry.Subject = "This is a test appointment"
  CEntry.Location = "Mettmann"
  CEntry.Categories = "Test;test1;test2"
  Set CEntry.StartDT = New NotesDateTime("11.07.2008 13:00:00") 
  Set CEntry.EndDT = New NotesDateTime("11.07.2008 14:00:00") 
   
  Msgbox CEntry.CreateSingleEntry
   
End Sub
```


## Why do we use it?
It is a long established fact that a reader will be distracted by the readable content of a page when looking at its layout. The point of using Lorem Ipsum is that it has a more-or-less normal distribution of letters, as opposed to using 'Content here, content here', making it look like readable English. Many desktop publishing packages and web page editors now use Lorem Ipsum as their default model text, and a search for 'lorem ipsum' will uncover many web sites still in their infancy. Various versions have evolved over the years, sometimes by accident, sometimes on purpose (injected humour and the like).

## Where does it come from?
Contrary to popular belief, Lorem Ipsum is not simply random text. It has roots in a piece of classical Latin literature from 45 BC, making it over 2000 years old. Richard McClintock, a Latin professor at Hampden-Sydney College in Virginia, looked up one of the more obscure Latin words, consectetur, from a Lorem Ipsum passage, and going through the cites of the word in classical literature, discovered the undoubtable source. Lorem Ipsum comes from sections 1.10.32 and 1.10.33 of "de Finibus Bonorum et Malorum" (The Extremes of Good and Evil) by Cicero, written in 45 BC. This book is a treatise on the theory of ethics, very popular during the Renaissance. The first line of Lorem Ipsum, "Lorem ipsum dolor sit amet..", comes from a line in section 1.10.32.
