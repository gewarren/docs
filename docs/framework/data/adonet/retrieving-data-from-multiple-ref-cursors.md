---
description: "Learn more about retrieving data from multiple REF CURSORs using an OracleDataReader by studying an example that executes a PL/SQL stored procedure."
title: "Retrieving Data from Multiple REF CURSORs Using an OracleDataReader"
ms.date: "03/30/2017"
dev_langs:
  - "vb"
---
# Retrieving Data from Multiple REF CURSORs Using an OracleDataReader

This Visual Basic example executes a PL/SQL stored procedure that returns two REF CURSOR parameters, and reads the values using an <xref:System.Data.OracleClient.OracleDataReader>.

```vb
Private Sub Button1_Click( _
  ByVal sender As Object, ByVal e As System.EventArgs)  _
  Handles Button1.Click

  Dim connString As New String("...")
  Using conn As New OracleConnection(connString)
    Dim cmd As New OracleCommand()
    Dim rdr As OracleDataReader

    conn.Open()
    cmd.Connection = conn
    cmd.CommandText = "CURSPKG.OPEN_TWO_CURSORS"
    cmd.CommandType = CommandType.StoredProcedure
    cmd.Parameters.Add(New OracleParameter( _
      "EMPCURSOR", OracleType.Cursor)).Direction = _
      ParameterDirection.Output
    cmd.Parameters.Add(New OracleParameter(_
      "DEPTCURSOR", OracleType.Cursor)).Direction = _
      ParameterDirection.Output

    rdr = cmd.ExecuteReader(CommandBehavior.CloseConnection)
    While (rdr.Read())
        REM do something with the values from the EMP table
    End While

    rdr.NextResult()
    While (rdr.Read())
        REM do something with the values from the DEPT table
    End While
    rdr.Close()
  End Using
End Sub
```

## See also

- [Oracle REF CURSORs](oracle-ref-cursors.md)
- [ADO.NET Overview](ado-net-overview.md)
