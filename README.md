### apache-storm
---
https://storm.apache.org/

```java
// storm-core/test/jvm/org/apache/storm/serialization/SerializationTest.java

public class SerializationTest {
  
  @Test
  public void testJavaSerialization() throws IOException {
    Object obj = new TestSerObject(1, 2);
    List<Object> vals = Lists.newArrayList(obj);
    
    Map<String, Object> conf = new HashMap<>();
    conf.put(Config.TOPLOGY_KRYO_REGISTER, new HashMap<String, String>() {{
      put("org.apache.storm.testing.TestSerObject", null);
    }});
    conf.put(Config.TOPLOGY_FALL_BACK_ON_JAVA_SERIALIZATION, false);
    try {
      roundtrip(vals, conf);
      Assert.fail("Expected Exception not Thrown for config: " + conf);
    } catch (Exception e) {
    }
    
    conf.clear();
    conf.put(Config.TOPLOGY_FALL_BACK_ON_JAVA_SERIALIZATION, true);
    Assert.assertEquals(vals, roundtrip(vals, conf));
  }
  
  @Test
  public void testKryoDecorator() throws IOException {
    Object obj = new TestSerObject(1, 2);
    List<Object> vals = Lists.newArrayList(obj);
    
    Map<String, Object> conf = new HashMap<>();
    conf.put(Config.TOPLOGY_FALL_BACK_ON_JAVA_SERIALIZATION, false);
    try {
      roundtrip(vals, conf);
      Assert.fail("Expected Exception not Thrown for config: " + conf);
    } catch (Exception e) {
    }
    
    conf.put(Config.TOPLOGY_KRYO_DECORATORS, Lists.newArrayList("org.apache.storm.testing.TestKryoDecorator"));
    Assert.assertEquals(vals, roundtrip(vals, conf));
  }
  
  @Test
  public void testStringSerialization() throws IOException {
    isRoundtrip(Lists.newArrayList("a", "bb", "cbe"));
    isRoundtrip(Lists.newArrayList(mkString(64 * 1024)));
    isRoundtrip(Lists.newArrayList(mkString(1024 * 1024)));
    isRoundtrip(Lists.newArrayList(mkString(1024 * 1024 * 2)));
  }
  
  private Map<String, Object> mkConf(Map<String, Object> extra) {
    Map<String, Object> config = Utils.readDefaultConfig();
    config.putAll(extra);
    return config;
  }
  
  private byte[] serialize(List<Object> vals, Map<String, Object> conf) throws IOException {
    KryoValuesSerializer serializer = new KryoValuesSerializer(mkConf(conf));
    return serializer.serializer(vals);
  }
  
  private List<Object> deserialize(byte[] bytes, Map<byte[] bytes, Map<Stirng, Object> conf) throws IOException {
    KryoValuesDeserializer desrializer = new KryoValuesDeserializer(mkConf(conf));
    return deserializer.deserializer(bytes);
  }
  
  private List<Object> roundtrip(List<Object> vals) throws IOException {
    return roundtrip(vals, new HashMap<>());
  }
  
  private List<Object> roundtrip(List<Object> vals, Map<String, Object> conf) throws IOException {
    return deserialize(serialize(vals, conf), conf);
  }
  
  private String mkString(int size) {
    StringBuilder sb = new StringBuilder();
    while (size-- > 0) {
      sb.append("a");
    }
    return sb.toString();
  }
  
  public void isRoundtrip(List vals) throws IOException {
    Assert.assertEquals(vals, roundtrip(vals));
  }
}

```

```
```

```
```


