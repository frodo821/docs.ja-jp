### <a name="netdatacontractserializer-fails-to-deserialize-a-concurrentdictionary-serialized-with-a-different-net-version"></a>異なる .NET バージョンでシリアル化された ConcurrentDictionary を NetDataContractSerializer で逆シリアル化できない

|   |   |
|---|---|
|説明|設計上、<xref:System.Runtime.Serialization.NetDataContractSerializer?displayProperty=name> は、シリアル化と逆シリアル化の両方で、同一の CLR 型を共有する結果になる場合にのみ使用できます。 そのため、.NET Framework の 1 つのバージョンでシリアル化されたオブジェクトは、異なるバージョンで逆シリアル化できるない可能性があります。 .NET Framework 4.5 以前でシリアル化され、.NET Framework 4.5.1 以降で逆シリアル化された場合、<xref:System.Collections.Concurrent.ConcurrentDictionary%602?displayProperty=name> の型は正しく逆シリアル化されないと認識されています。|
|提案される解決策|この問題の考えられるいくつかの回避策を以下に示します。<ul><li>シリアル化するコンピューターをアップグレードして、.NET Framework 4.5.1 も使用するようにします。</li><li>シリアル化と逆シリアル化の両方で CLT 型がまったく同じになることが想定されない場合は、<xref:System.Runtime.Serialization.NetDataContractSerializer?displayProperty=name> ではなく、<xref:System.Runtime.Serialization.DataContractSerializer?displayProperty=name> を使用します。</li><li>この特定の 4.5-&gt;4.5.1 の問題は見られないため、<xref:System.Collections.Concurrent.ConcurrentDictionary%602?displayProperty=name> ではなく、<xref:System.Collections.Generic.Dictionary%602?displayProperty=name> を使用します。</li></ul>|
|スコープ|マイナー|
|Version|4.5.1|
|型|ランタイム|
|影響を受ける API|<ul><li><xref:System.Runtime.Serialization.NetDataContractSerializer.Deserialize(System.IO.Stream)?displayProperty=nameWithType></li></ul>|

