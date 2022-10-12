# springboot-specification-predicate-method

## Repository
 
Extend JpaSpecificationExecutor<ObjectClass>

    public interface MaterialReceiptRepository extends JpaRepository<MaterialReceiptEntity, Integer>, 
      JpaSpecificationExecutor<MaterialReceiptEntity>{


      }

## Search 

Search Method

    public PageMaterialReceiptEntity searchByConditions(Integer id, String receiptCode, String plateNumber
          , Integer specificationId, Integer supplierId, Integer storageId, String billOfLandingCode,
          String deliveryTicketCode, Integer typeId, String status, Integer sourceId, Integer createrId,
          Integer factoryId, Integer stationId, Integer receiptTypeId, Integer transactionTypeId, 
          Integer pageNum, Boolean removed, Long createTime, Long completeTime) {
        Specification<MaterialReceiptEntity> specification = new Specification<MaterialReceiptEntity>() {

          @Override
          public Predicate toPredicate(Root<MaterialReceiptEntity> root, CriteriaQuery<?> query,
              CriteriaBuilder criteriaBuilder) {
            List<Predicate> predicates = new ArrayList<>();

            if(null!=id)
              predicates.add(criteriaBuilder.equal(root.get("id"), id));
            if(null!=receiptCode && !receiptCode.equals(""))
              predicates.add(criteriaBuilder.like(root.get("receiptCode"), "%"+receiptCode+"%"));
            if(null!=plateNumber && !plateNumber.equals(""))
              predicates.add(criteriaBuilder.like(root.get("plateNumber"), "%"+plateNumber+"%"));
            if(null!=specificationId)
              predicates.add(criteriaBuilder.equal(root.get("specificationId"), specificationId));
            if(null!=supplierId)
              predicates.add(criteriaBuilder.equal(root.get("supplierId"), supplierId));
            if(null!=storageId)
              predicates.add(criteriaBuilder.equal(root.get("storageId"), storageId));
            if(null!=billOfLandingCode && !billOfLandingCode.equals(""))
              predicates.add(criteriaBuilder.like(root.get("billOfLandingCode"), "%"+billOfLandingCode+"%"));
            if(null!=deliveryTicketCode && !deliveryTicketCode.equals(""))
              predicates.add(criteriaBuilder.like(root.get("deliveryTicketCode"), "%"+deliveryTicketCode+"%"));
            if(null!=typeId)
              predicates.add(criteriaBuilder.equal(root.get("typeId"), typeId));
            if(null!=status && !status.equals(""))
              predicates.add(criteriaBuilder.like(root.get("status"), status));
            if(null!=sourceId)
              predicates.add(criteriaBuilder.equal(root.get("sourceId"), sourceId));
            if(null!=createrId)
              predicates.add(criteriaBuilder.equal(root.get("createrId"), createrId));
            if(null!=factoryId)
              predicates.add(criteriaBuilder.equal(root.get("factoryId"), factoryId));
            if(null!=stationId)
              predicates.add(criteriaBuilder.equal(root.get("stationId"), stationId));
            if(null!=receiptTypeId)
              predicates.add(criteriaBuilder.equal(root.get("receiptTypeId"), receiptTypeId));
            if(null!=transactionTypeId)
              predicates.add(criteriaBuilder.equal(root.get("transactionTypeId"), transactionTypeId));
            if(null!=removed)
              predicates.add(criteriaBuilder.equal(root.get("removed"), removed));
            if(null!=createTime) {
              predicates.add(criteriaBuilder.between(root.get("createTime"), 
                  dateUtil.setDateToBeginOfDay(new Date(createTime)), 
                  dateUtil.setDateToEndofDay(new Date(createTime))));
            }
            if(null!=completeTime) {
              predicates.add(criteriaBuilder.between(root.get("receiptCompleteTime"), 
                  dateUtil.setDateToBeginOfDay(new Date(createTime)), 
                  dateUtil.setDateToEndofDay(new Date(createTime))));
            }

            return criteriaBuilder.and(predicates.toArray(new Predicate[predicates.size()]));
          }
        };

        PageRequest page = PageRequest.of(pageNum, 10);
        Page<MaterialReceiptEntity> pgResult = materialReceiptRepository.findAll(specification, page);
        Integer maxPageInteger = pgResult.getTotalPages();

        PageMaterialReceiptEntity result = new PageMaterialReceiptEntity(pgResult.getContent(), maxPageInteger);

        return result;
      }
