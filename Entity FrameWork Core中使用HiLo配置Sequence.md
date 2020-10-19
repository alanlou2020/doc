## Entity FrameWork Core中使用HiLo配置Sequence

### 1、在OnModelCreating方法中配置IncrementsBy，并指明用户模式和Sequence

~~~ 
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
	base.OnModelCreating(modelBuilder);
	modelBuilder.HasDefaultSchema("RTUSER");

	/*如果要启用HiLo来配置Oracle的Sequence需到Oracle数据库中，
    配置Sequence的“Increment by”和“Cache size”(我是把“Increment by”和“Cache size”都设置成了20)，
    Hilo的设置为IncrementsBy(20)（注意：这里要小于等数据库中配置的“Increment by”）*/
    modelBuilder.HasSequence<int>("SEQ_ZX_HOSPERPGOODS_REL_VENDOR")
              .StartsAt(1).IncrementsBy(20);
    modelBuilder.Entity<ZX_HOSPERPGOODS_REL_VENDOR>(entity =>
    {
        entity.ToTable("ZX_HOSPERPGOODS_REL_VENDOR");
        entity.Property(o => o.ID).UseHiLo("SEQ_ZX_HOSPERPGOODS_REL_VENDOR", "RTUSER");
    });
}
~~~

### 2、在Oracle中配置Sequence的“Increment by”和“Cache size”，数据库中的“Increment by”要比EF Core中“IncrementsBy”的值大，否则获取的Sequence会重复。

### 3、原理：HiLo配置Sequence可以减少对数据库的访问次数，在访问一次数据库获取当前Sequence值后，如果EF Core设置了HiLo的IncrementsBy的值，在增长完IncrementsBy的值得次数后，再去访问数据库获取最新的Sequence，因此，数据库中的“Increment by”要比EF Core中“IncrementsBy”的值大，否则获取的Sequence会重复，导致主键重复错误的发生。