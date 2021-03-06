---
title: バイナリ シリアル化
ms.date: 01/02/2018
helpviewer_keywords:
- binary serialization
- serialization, about serialization
- deserialization
- binary serialization, about serialization
- binary serialization, .net core serialization
- serialization, cross-framework
ms.assetid: 2b1ea3be-1152-4032-b2b3-07794054c405
author: ViktorHofer
ms.author: mairaw
ms.openlocfilehash: 63158dd2dda5388870d3d5878fe29cb004aec401
ms.sourcegitcommit: 3d5d33f384eeba41b2dff79d096f47ccc8d8f03d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/04/2018
---
# <a name="binary-serialization"></a>バイナリ シリアル化

シリアル化は、オブジェクトの状態をストレージ メディアに格納するプロセスとして定義することができます。 このプロセスの実行中に、オブジェクトのパブリックおよびプライベートなフィールドとクラス (クラスを格納しているアセンブリを含む) の名前がバイト ストリームに変換され、データ ストリームに書き込まれます。 続いてオブジェクトが逆シリアル化され、元のオブジェクトの完全な複製が作成されます。

オブジェクト指向環境でシリアル化機構を実装する場合は、使いやすさと柔軟性の間での数多くのトレードオフについて考慮する必要があります。 プロセスを十分に制御できる場合は、プロセスの大部分を自動化できます。 たとえば、単純なバイナリ シリアル化では不十分な状況が発生する場合や、シリアル化が必要なクラス内のフィールドを決定するだけの明確な理由がある場合があります。 以下のセクションでは、.NET に用意されている堅牢なシリアル化機構について検討し、必要に応じてプロセスをカスタマイズするためのいくつかの重要な機能について説明します。

> [!NOTE]
> オブジェクトのシリアル化と逆シリアル化を行う際に使用した .NET Framework のバージョンが異なる場合、UTF-8 または UTF-7 でエンコードされたオブジェクトの状態は保持されません。

[!INCLUDE [binary-serialization-warning](../../../includes/binary-serialization-warning.md)]

バイナリ シリアル化の性質上、オブジェクト内でのプライベート メンバーの変更が可能であり、その状態が変化するため、パブリック API サーフェイスで動作する JSON.NET のような他のシリアル化フレームワークを使用することをお勧めします。

## <a name="binary-serialization-in-net-core"></a>.NET Core でのバイナリ シリアル化

.NET Core では、型のサブセットでのバイナリ シリアル化がサポートされます。 サポートされている型のリストは、「[シリアル化可能な型](#serializable-types)」で確認できます。 定義済みの一連の型は、.NET Framework 4.5.1 以降のバージョンと .NET Core 2.0 以降のバージョン間でシリアル化できることが保証されていいます。 Mono などの他の .NET 実装は公式にサポートされておらず、また機能もしません。

### <a name="serializable-types"></a>シリアル化可能な型

- <xref:Microsoft.CSharp.RuntimeBinder.RuntimeBinderException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:Microsoft.CSharp.RuntimeBinder.RuntimeBinderInternalCompilerException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.AccessViolationException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.AggregateException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.AppDomainUnloadedException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.ApplicationException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.ArgumentException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.ArgumentNullException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.ArgumentOutOfRangeException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.ArithmeticException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.Array?displayProperty=nameWithType>   
- <xref:System.ArraySegment%601?displayProperty=nameWithType>   
- <xref:System.ArrayTypeMismatchException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.Attribute?displayProperty=nameWithType>
- <xref:System.BadImageFormatException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.Boolean?displayProperty=nameWithType>   
- <xref:System.Byte?displayProperty=nameWithType>   
- <xref:System.CannotUnloadAppDomainException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.Char?displayProperty=nameWithType>   
- <xref:System.Collections.ArrayList?displayProperty=nameWithType>   
- <xref:System.Collections.BitArray?displayProperty=nameWithType>   
- <xref:System.Collections.Comparer?displayProperty=nameWithType>   
- <xref:System.Collections.DictionaryEntry?displayProperty=nameWithType>   
- <xref:System.Collections.Generic.Comparer%601?displayProperty=nameWithType>   
- <xref:System.Collections.Generic.Dictionary%602?displayProperty=nameWithType>   
- <xref:System.Collections.Generic.EqualityComparer%601?displayProperty=nameWithType>   
- <xref:System.Collections.Generic.HashSet%601?displayProperty=nameWithType>   
- <xref:System.Collections.Generic.KeyNotFoundException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.Collections.Generic.KeyValuePair%602?displayProperty=nameWithType>   
- <xref:System.Collections.Generic.LinkedList%601?displayProperty=nameWithType>   
- <xref:System.Collections.Generic.List%601?displayProperty=nameWithType>   
- <xref:System.Collections.Generic.Queue%601?displayProperty=nameWithType>   
- <xref:System.Collections.Generic.SortedDictionary%602?displayProperty=nameWithType>   
- <xref:System.Collections.Generic.SortedList%602?displayProperty=nameWithType>   
- <xref:System.Collections.Generic.SortedSet%601?displayProperty=nameWithType>   
- <xref:System.Collections.Generic.Stack%601?displayProperty=nameWithType>   
- <xref:System.Collections.Hashtable?displayProperty=nameWithType>   
- <xref:System.Collections.ObjectModel.Collection%601?displayProperty=nameWithType>   
- <xref:System.Collections.ObjectModel.KeyedCollection%602?displayProperty=nameWithType>   
- <xref:System.Collections.ObjectModel.ObservableCollection%601?displayProperty=nameWithType>   
- <xref:System.Collections.ObjectModel.ReadOnlyCollection%601?displayProperty=nameWithType>   
- <xref:System.Collections.ObjectModel.ReadOnlyDictionary%602?displayProperty=nameWithType>   
- <xref:System.Collections.ObjectModel.ReadOnlyObservableCollection%601?displayProperty=nameWithType>   
- <xref:System.Collections.Queue?displayProperty=nameWithType>   
- <xref:System.Collections.SortedList?displayProperty=nameWithType>   
- <xref:System.Collections.Specialized.HybridDictionary?displayProperty=nameWithType>   
- <xref:System.Collections.Specialized.ListDictionary?displayProperty=nameWithType>   
- <xref:System.Collections.Specialized.OrderedDictionary?displayProperty=nameWithType>   
- <xref:System.Collections.Specialized.StringCollection?displayProperty=nameWithType>   
- <xref:System.Collections.Specialized.StringDictionary?displayProperty=nameWithType>   
- <xref:System.Collections.Stack?displayProperty=nameWithType>   
- `System.Collections.Generic.NonRandomizedStringEqualityComparer` <!--zz <xref:System.Collections.Generic.NonRandomizedStringEqualityComparer?displayProperty=nameWithType> --> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.ComponentModel.BindingList%601?displayProperty=nameWithType>   
- <xref:System.ComponentModel.DataAnnotations.ValidationException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.ComponentModel.Design.CheckoutException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.ComponentModel.InvalidAsynchronousStateException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.ComponentModel.InvalidEnumArgumentException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.ComponentModel.LicenseException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用でき、.NET Core への .NET Framework からシリアル化はサポートされていません)
- <xref:System.ComponentModel.WarningException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.ComponentModel.Win32Exception?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.Configuration.ConfigurationErrorsException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.Configuration.ConfigurationException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.Configuration.Provider.ProviderException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.Configuration.SettingsPropertyIsReadOnlyException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.Configuration.SettingsPropertyNotFoundException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.Configuration.SettingsPropertyWrongTypeException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.ContextMarshalException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.DBNull?displayProperty=nameWithType> (.NET Core 2.0.2 とそれ以降のバージョンで使用可能)   
- <xref:System.Data.Common.DbException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.Data.ConstraintException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.Data.DBConcurrencyException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.Data.DataException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.Data.DataSet?displayProperty=nameWithType>
- <xref:System.Data.DataTable?displayProperty=nameWithType> (RemotingFormat SerializationFormat.Binary を設定しない限り場合のみ交換できます .NET Core 2.1 とそれ以降のバージョンを使用します。)   
- <xref:System.Data.DeletedRowInaccessibleException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.Data.DuplicateNameException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.Data.EvaluateException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.Data.InRowChangingEventException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.Data.InvalidConstraintException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.Data.InvalidExpressionException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.Data.MissingPrimaryKeyException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.Data.NoNullAllowedException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.Data.Odbc.OdbcException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.Data.OperationAbortedException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.Data.PropertyCollection?displayProperty=nameWithType>   
- <xref:System.Data.ReadOnlyException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.Data.RowNotInTableException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.Data.SqlClient.SqlException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用でき、.NET Core への .NET Framework からシリアル化はサポートされていません)
- <xref:System.Data.SqlTypes.SqlAlreadyFilledException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.Data.SqlTypes.SqlBoolean?displayProperty=nameWithType>   
- <xref:System.Data.SqlTypes.SqlByte?displayProperty=nameWithType>   
- <xref:System.Data.SqlTypes.SqlDateTime?displayProperty=nameWithType>   
- <xref:System.Data.SqlTypes.SqlDouble?displayProperty=nameWithType>   
- <xref:System.Data.SqlTypes.SqlGuid?displayProperty=nameWithType>   
- <xref:System.Data.SqlTypes.SqlInt16?displayProperty=nameWithType>   
- <xref:System.Data.SqlTypes.SqlInt32?displayProperty=nameWithType>   
- <xref:System.Data.SqlTypes.SqlInt64?displayProperty=nameWithType>   
- <xref:System.Data.SqlTypes.SqlNotFilledException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.Data.SqlTypes.SqlNullValueException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.Data.SqlTypes.SqlString?displayProperty=nameWithType>   
- <xref:System.Data.SqlTypes.SqlTruncateException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.Data.SqlTypes.SqlTypeException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.Data.StrongTypingException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.Data.SyntaxErrorException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.Data.VersionNotFoundException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.DataMisalignedException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.DateTime?displayProperty=nameWithType>   
- <xref:System.DateTimeOffset?displayProperty=nameWithType>   
- <xref:System.Decimal?displayProperty=nameWithType>   
- `System.Diagnostics.Contracts.ContractException` <!--zz <xref:System.Diagnostics.Contracts.ContractException?displayProperty=nameWithType> --> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.Diagnostics.Tracing.EventSourceException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.IO.DirectoryNotFoundException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.DirectoryServices.AccountManagement.MultipleMatchesException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.DirectoryServices.AccountManagement.NoMatchingPrincipalException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.DirectoryServices.AccountManagement.PasswordException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.DirectoryServices.AccountManagement.PrincipalException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.DirectoryServices.AccountManagement.PrincipalExistsException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.DirectoryServices.AccountManagement.PrincipalOperationException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.DirectoryServices.AccountManagement.PrincipalServerDownException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.DirectoryServices.ActiveDirectory.ActiveDirectoryObjectExistsException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.DirectoryServices.ActiveDirectory.ActiveDirectoryObjectNotFoundException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.DirectoryServices.ActiveDirectory.ActiveDirectoryOperationException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.DirectoryServices.ActiveDirectory.ActiveDirectoryServerDownException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.DirectoryServices.ActiveDirectory.ForestTrustCollisionException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.DirectoryServices.ActiveDirectory.SyncFromAllServersOperationException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.DirectoryServices.DirectoryServicesCOMException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.DirectoryServices.Protocols.BerConversionException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.DirectoryServices.Protocols.DirectoryException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.DirectoryServices.Protocols.DirectoryOperationException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.DirectoryServices.Protocols.LdapException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.DirectoryServices.Protocols.TlsOperationException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.DivideByZeroException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.DllNotFoundException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.Double?displayProperty=nameWithType>   
- <xref:System.Drawing.Color?displayProperty=nameWithType>   
- <xref:System.Drawing.Point?displayProperty=nameWithType>   
- <xref:System.Drawing.PointF?displayProperty=nameWithType>   
- <xref:System.Drawing.Rectangle?displayProperty=nameWithType>   
- <xref:System.Drawing.RectangleF?displayProperty=nameWithType>   
- <xref:System.Drawing.Size?displayProperty=nameWithType>   
- <xref:System.Drawing.SizeF?displayProperty=nameWithType>   
- <xref:System.DuplicateWaitObjectException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.EntryPointNotFoundException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.Enum?displayProperty=nameWithType>   
- <xref:System.EventArgs?displayProperty=nameWithType> (.NET Core 2.0.6 以降で使用可能)
- <xref:System.Exception?displayProperty=nameWithType>   
- <xref:System.ExecutionEngineException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.FieldAccessException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.FormatException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.Globalization.CompareInfo?displayProperty=nameWithType>   
- <xref:System.Globalization.CultureNotFoundException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.Globalization.SortVersion?displayProperty=nameWithType>   
- <xref:System.Guid?displayProperty=nameWithType>   
- `System.IO.Compression.ZLibException` <!--zz <xref:System.IO.Compression.ZLibException?displayProperty=nameWithType --> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.IO.DriveNotFoundException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.IO.EndOfStreamException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.IO.FileFormatException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.IO.FileLoadException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.IO.FileNotFoundException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.IO.IOException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.IO.InternalBufferOverflowException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.IO.InvalidDataException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.IO.IsolatedStorage.IsolatedStorageException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.IO.PathTooLongException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.IndexOutOfRangeException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.InsufficientExecutionStackException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.InsufficientMemoryException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.Int16?displayProperty=nameWithType>   
- <xref:System.Int32?displayProperty=nameWithType>   
- <xref:System.Int64?displayProperty=nameWithType>   
- <xref:System.IntPtr?displayProperty=nameWithType>   
- <xref:System.InvalidCastException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.InvalidOperationException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.InvalidProgramException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.InvalidTimeZoneException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.MemberAccessException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.MethodAccessException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.MissingFieldException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.MissingMemberException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.MissingMethodException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.MulticastNotSupportedException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.Net.Cookie?displayProperty=nameWithType>   
- <xref:System.Net.CookieCollection?displayProperty=nameWithType>   
- <xref:System.Net.CookieContainer?displayProperty=nameWithType>   
- <xref:System.Net.CookieException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.Net.HttpListenerException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.Net.Mail.SmtpException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.Net.Mail.SmtpFailedRecipientException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.Net.Mail.SmtpFailedRecipientsException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.Net.NetworkInformation.NetworkInformationException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.Net.NetworkInformation.PingException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.Net.ProtocolViolationException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.Net.Sockets.SocketException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.Net.WebException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.Net.WebSockets.WebSocketException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.NotFiniteNumberException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.NotImplementedException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.NotSupportedException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.NullReferenceException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.Nullable%601?displayProperty=nameWithType>   
- <xref:System.Numerics.BigInteger?displayProperty=nameWithType>   
- <xref:System.Numerics.Complex?displayProperty=nameWithType>   
- <xref:System.Object?displayProperty=nameWithType>   
- <xref:System.ObjectDisposedException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.OperationCanceledException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.OutOfMemoryException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.OverflowException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.PlatformNotSupportedException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.RankException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.Reflection.AmbiguousMatchException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.Reflection.CustomAttributeFormatException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.Reflection.InvalidFilterCriteriaException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.Reflection.ReflectionTypeLoadException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用でき、.NET Core への .NET Framework からシリアル化はサポートされていません)
- <xref:System.Reflection.TargetException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.Reflection.TargetInvocationException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.Reflection.TargetParameterCountException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.Resources.MissingManifestResourceException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.Resources.MissingSatelliteAssemblyException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.Runtime.CompilerServices.RuntimeWrappedException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.Runtime.InteropServices.COMException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.Runtime.InteropServices.ExternalException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.Runtime.InteropServices.InvalidComObjectException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.Runtime.InteropServices.InvalidOleVariantTypeException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.Runtime.InteropServices.MarshalDirectiveException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.Runtime.InteropServices.SEHException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.Runtime.InteropServices.SafeArrayRankMismatchException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.Runtime.InteropServices.SafeArrayTypeMismatchException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.Runtime.Serialization.InvalidDataContractException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.Runtime.Serialization.SerializationException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.SByte?displayProperty=nameWithType>   
- <xref:System.Security.AccessControl.PrivilegeNotHeldException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.Security.Authentication.AuthenticationException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.Security.Authentication.InvalidCredentialException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.Security.Cryptography.CryptographicException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.Security.Cryptography.CryptographicUnexpectedOperationException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- `System.Security.Cryptography.Xml.CryptoSignedXmlRecursionException` <!--zz <xref:System.Security.Cryptography.Xml.CryptoSignedXmlRecursionException?displayProperty=nameWithType --> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.Security.HostProtectionException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.Security.Policy.PolicyException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.Security.Principal.IdentityNotMappedException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.Security.SecurityException?displayProperty=nameWithType> (.NET Core 2.0.4 およびそれ以降のバージョン、制限付きのシリアル化データで使用可能)
- <xref:System.Security.VerificationException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.Security.XmlSyntaxException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.ServiceProcess.TimeoutException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.Single?displayProperty=nameWithType>   
- <xref:System.StackOverflowException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.String?displayProperty=nameWithType>   
- <xref:System.StringComparer?displayProperty=nameWithType>   
- <xref:System.SystemException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.Text.DecoderFallbackException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.Text.EncoderFallbackException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.Text.RegularExpressions.RegexMatchTimeoutException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.Text.StringBuilder?displayProperty=nameWithType>   
- <xref:System.Threading.AbandonedMutexException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.Threading.BarrierPostPhaseException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.Threading.LockRecursionException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.Threading.SemaphoreFullException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.Threading.SynchronizationLockException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.Threading.Tasks.TaskCanceledException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.Threading.Tasks.TaskSchedulerException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.Threading.ThreadAbortException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.Threading.ThreadInterruptedException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.Threading.ThreadStartException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.Threading.ThreadStateException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.Threading.WaitHandleCannotBeOpenedException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.TimeSpan?displayProperty=nameWithType>   
- <xref:System.TimeZoneInfo.AdjustmentRule?displayProperty=nameWithType>   
- <xref:System.TimeZoneInfo?displayProperty=nameWithType>   
- <xref:System.TimeZoneNotFoundException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.TimeoutException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.Transactions.TransactionAbortedException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.Transactions.TransactionException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.Transactions.TransactionInDoubtException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.Transactions.TransactionManagerCommunicationException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.Transactions.TransactionPromotionException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.Tuple?displayProperty=nameWithType>   
- <xref:System.TypeAccessException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.TypeInitializationException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.TypeLoadException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.TypeUnloadedException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.UInt16?displayProperty=nameWithType>   
- <xref:System.UInt32?displayProperty=nameWithType>   
- <xref:System.UInt64?displayProperty=nameWithType>   
- <xref:System.UIntPtr?displayProperty=nameWithType>   
- <xref:System.UnauthorizedAccessException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.Uri?displayProperty=nameWithType>   
- <xref:System.UriFormatException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.ValueTuple?displayProperty=nameWithType> (いない .NET Framework 4.7 と以前のバージョンでシリアル化可能)  
- <xref:System.ValueType?displayProperty=nameWithType>   
- <xref:System.Version?displayProperty=nameWithType>   
- <xref:System.WeakReference%601?displayProperty=nameWithType>   
- <xref:System.WeakReference?displayProperty=nameWithType>   
- <xref:System.Xml.Schema.XmlSchemaException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.Xml.Schema.XmlSchemaInferenceException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.Xml.Schema.XmlSchemaValidationException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.Xml.XPath.XPathException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.Xml.XmlException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.Xml.Xsl.XsltCompileException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)
- <xref:System.Xml.Xsl.XsltException?displayProperty=nameWithType> (.NET Core 2.0.4 とそれ以降のバージョンで使用可能)

## <a name="in-this-section"></a>このセクションの内容

 [シリアル化の概念](../../../docs/standard/serialization/serialization-concepts.md)  
 データをストレージに永続化するシナリオと、アプリケーション ドメインを越えてオブジェクトを受け渡しするシナリオの 2 つを例として、シリアル化の有効な利用方法について説明します。  
  
 [基本的なシリアル化](../../../docs/standard/serialization/basic-serialization.md)  
 バイナリ フォーマッタと SOAP フォーマッタを使用してオブジェクトをシリアル化する方法について説明します。  
  
 [選択的シリアル化](../../../docs/standard/serialization/selective-serialization.md)  
 クラスの一部のメンバーがシリアル化されないようにする方法について説明します。  
  
 [カスタムのシリアル化](../../../docs/standard/serialization/custom-serialization.md)  
 <xref:System.Runtime.Serialization.ISerializable> インターフェイスを使用してクラスのシリアル化をカスタマイズする方法について説明します。  
  
 [シリアル化プロセスの手順](../../../docs/standard/serialization/steps-in-the-serialization-process.md)  
 フォーマッタで <xref:System.Runtime.Serialization.Formatter.Serialize%2A> メソッドを呼び出した場合のシリアル化方法について説明します。  
  
 [バージョン トレラントなシリアル化](../../../docs/standard/serialization/version-tolerant-serialization.md)  
 アプリケーションに例外をスローさせることなく後から変更できる、シリアル化可能な型の作成方法について説明します。  
  
 [シリアル化のガイドライン](../../../docs/standard/serialization/serialization-guidelines.md)  
 オブジェクトをシリアル化するタイミングを決定するのに役立つ、いくつかの一般的なガイドラインを示します。  
  
## <a name="reference"></a>参照  
 <xref:System.Runtime.Serialization>  
 オブジェクトのシリアル化と逆シリアル化に使用できるクラスが含まれています。  
  
## <a name="related-sections"></a>関連項目  
 [XML シリアル化および SOAP シリアル化](../../../docs/standard/serialization/xml-and-soap-serialization.md)  
 共通言語ランタイムに付属している XML シリアル化機構について説明します。  
  
 [セキュリティとシリアル化](../../../docs/framework/misc/security-and-serialization.md)  
 シリアル化を実行するコードを記述する際に従う必要がある、安全なコーディングのガイドラインについて説明します。  
  
 [リモート オブジェクト](https://msdn.microsoft.com/library/515686e6-0a8d-42f7-8188-73abede57c58)  
 .NET Framework でリモート通信に利用できるさまざまな通信方法について説明します。  
  
 [ASP.NET と XML Web サービス クライアントを使用して作成した XML Web サービス](https://msdn.microsoft.com/library/1e64af78-d705-4384-b08d-591a45f4379c)  
 ASP.NET を使用して作成した XML Web サービスのプログラミング方法について説明するトピックを示します。
