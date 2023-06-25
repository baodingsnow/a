> 在做软件库项目的时候，遇到一个问题
>
> 拿soa举例      展示soa列表功能和导出soa列表功能最后调用的是一个sql   即 
>
> ```sql
> select  s.soa信息,s.soaversion信息
> from soa s
> left join   soa_version  sv  on s.id =sv.soaId 
> ```





列表功能需要分页

刚开始我想的是写两个不同的service  一个 分页  一个不分页

到最后就有这样的结果，两个mapper关联一个mapper.xml   

总之这样写很不好  为什么不好后续再写



> 所以功能就得这么写
>
> ```java
> public interface  DemoMapper{
>     Page<soaVo>   selectSoa(Dto,dto);
> }
> ```
>
> 
>
> ```java
> public interface DemoService{
>     Page<soaVo>   selectSoa(Dto,dto);
>     
> }
> ```
>
> ```java
> public class DemoServiceImpl extends ServiceImpl<SoaMapper, Soa> implements ISoaService{
>     Page<soaVo>   selectSoa(Dto,dto){
>         return  baseMapper.selectSoa(dto)
>     }
> }
> ```
>
> 
>
> ```java
> public class demoController {
>     @Autowired
>     private Demoservice demoservice;
>     
>     @GETmapping
>     public TableDataInfo<SoaVo> list(Dto  dto){
>         Ipage<MiddlewareVo>  middlewareVoPage= demoservice.selectSoa(dto);
>     }
>     
>     
>     @PostMapping
>     public void export(HttpServletResponse response, Dto dto){
>         List<BasicLibraryVo> list = basicLibraryService.selectBasicLibraryList(basicLibraryReqDto).getRecords();
>         ExcelUtil<BasicLibraryVo> util = new ExcelUtil<BasicLibraryVo>(BasicLibraryVo.class);
>         util.exportExcel(response, list, "基础库数据");
>     }
> }
> ```
>
> 

应为调用的service返回的是







