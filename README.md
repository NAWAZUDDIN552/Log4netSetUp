# Log4netSetUp
 First add the line under section tag:
  <section name="log4net" type="log4net.Config.Log4NetConfigurationSectionHandler,log4net" />
  
 <log4net>
    <appender name="SmtpAppender" type="log4net.Appender.SmtpAppender">
      <to value=""/>
      <from value=""/>
      <cc value=""/>
      <subject value="Logging Info"/>
      <smtpHost value="smtp.gmail.com"/>
      <authentication value ="basic"/>
      <username value=""/>
      <password value =""/>
      
      <enableSSL value ="true"/>
      <buffersize value="1"/>
      <layout type="log4net.Layout.PatternLayout"/>
      <conversionPattern value="%timestamp [%thread] %level - %message%newline%exception%newlineIdentity-%identity%newlineUsername-%username%newline"/>
      <!--<filter type="log4net.Filter.LevelRangeFilter">
        <levelMin value="ERROR" />
        <levelMax value="FATAL" />
        <acceptOnMatch value="true"/>
      </filter>-->
    </appender>
    <appender name="AdoNetAppender" type="log4net.Appender.AdoNetAppender">
      <bufferSize value="1" />
      <connectionType value="System.Data.SqlClient.SqlConnection, System.Data, Version=1.0.3300.0, Culture=neutral, PublicKeyToken=b77a5c561934e089"/>
      <connectionString value="data source=;initial catalog=;integrated security=false;persist security info=True;user id=;password="/>
      <commandText value="therecrm_admin.procLogs_Insert" />
      <commandType value="StoredProcedure" />
      <!--<commandText value="INSERT INTO tablename (logDate,logThread,logLevel,logSource,
    logMessage,exception) VALUES (@log_date, @log_thread, @log_level, 
    @log_source, @log_message, @exception)" />-->
      <parameter>
        <parameterName value="@Username" />
        <dbType value="AnsiString" />
        <size value="70" />
        <layout type="log4net.Layout.PatternLayout" value="%property{Username}"/>
        <!--<conversionPattern value="(%property{Username})" />-->
       
       
      </parameter>
        <!--<param name="ConversionPattern" value="%HTTPUser" />
        <converter>
          <name value="HTTPUser" />
          <type value="hbulens.Logging.HttpContextUserPatternConverter" />
        </converter>-->
       
      <parameter>
        <parameterName value="@log_date" />
        <dbType value="DateTime" />
        <layout type="log4net.Layout.RawTimeStampLayout" />
      </parameter>
      <parameter>
        <parameterName value="@log_thread" />
        <dbType value="AnsiString" />
        <size value="50" />
        <layout type="log4net.Layout.PatternLayout">
          <conversionPattern value="%thread" />
        </layout>
      </parameter>
      <parameter>
        <parameterName value="@log_level" />
        <dbType value="AnsiString" />
        <size value="50" />
        <layout type="log4net.Layout.PatternLayout">
          <conversionPattern value="%level" />
        </layout>
      </parameter>
      <parameter>
        <parameterName value="@log_source" />
        <dbType value="AnsiString" />
        <size value="300" />
        <layout type="log4net.Layout.PatternLayout">
          <conversionPattern value="%logger" />
        </layout>
      </parameter>
      <parameter>
        <parameterName value="@log_message" />
        <dbType value="AnsiString" />
        <size value="4000" />
        <layout type="log4net.Layout.PatternLayout">
          <conversionPattern value="%message" />
        </layout>
      </parameter>
      <parameter>
        <parameterName value="@exception" />
        <dbType value="AnsiString" />
        <size value="4000" />
        <layout type="log4net.Layout.ExceptionLayout" />
      </parameter>
    </appender>
    <appender name="RollingFileAppender" type="log4net.Appender.RollingFileAppender">
      <file value="C:\Log\RollingFileLog.txt" />
      <appendToFile value="true" />
      <rollingStyle value="Size" />
      <maximumFileSize value="10MB" />
      <maxSizeRollBackups value="5" />
      <staticLogFileName value="true" />
      <layout type="log4net.Layout.PatternLayout">
        <conversionPattern value="%date{ABSOLUTE} [%logger] %level - %message%newline%exception%newlineIdentity-%identity%newlineUsername-%username%newline" />
      </layout>
    </appender>

    <root>  
    <level value="All" />
      <!--<appender-ref ref="AdoNetAppender" />-->
      <!--<appender-ref ref="SmtpAppender" />
      <appender-ref ref="RollingFileAppender" />-->

    </root>  
</log4net>

Add CustomValues into AdoNetAppender:
            log4net.Config.XmlConfigurator.Configure();
            log4net.GlobalContext.Properties["Username"] = new HttpContextUserNameProvider();
public class HttpContextUserNameProvider
        {
            public override string ToString()
            {
                HttpContext context = HttpContext.Current;
                if (context != null && context.User != null && context.User.Identity.IsAuthenticated)
                {
                    return context.User.Identity.Name;
                }
                return "";
            }
        }			
            
